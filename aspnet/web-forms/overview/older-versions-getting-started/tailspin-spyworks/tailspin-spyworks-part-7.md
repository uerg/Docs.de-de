---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
title: 'Teil 7: Hinzufügen von Funktionen | Microsoft-Dokumentation'
author: JoeStagner
description: Dieser tutorialreihe werden alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen. Teil 7 fügt zusätzliche Funktionen wie das Konto übe...
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 50223ee9-11b9-4cf3-bca2-e2f10bf471f3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
msc.type: authoredcontent
ms.openlocfilehash: cada8d9aee649e4f2a5afc1ca2b46863ea458207
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823739"
---
<a name="part-7-adding-features"></a><span data-ttu-id="b85da-104">Teil 7: Hinzufügen von Features</span><span class="sxs-lookup"><span data-stu-id="b85da-104">Part 7: Adding Features</span></span>
====================
<span data-ttu-id="b85da-105">durch [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="b85da-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="b85da-106">Tailspin Spyworks wird veranschaulicht, wie außerordentlich einfach es ist, erstellen Sie leistungsstarke, skalierbare Anwendungen für die .NET-Plattform.</span><span class="sxs-lookup"><span data-stu-id="b85da-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="b85da-107">Es wird gezeigt, aus wie die hervorragenden neuen Funktionen in ASP.NET 4 zu verwenden, um eine online-Store, einschließlich der Warenkorb, Auschecken und Verwaltung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="b85da-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="b85da-108">Dieser tutorialreihe werden alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen.</span><span class="sxs-lookup"><span data-stu-id="b85da-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="b85da-109">Teil 7 fügt zusätzliche Funktionen wie das Konto überprüfen, produktbesprechungen und "beliebtesten Elemente" und "auch erworbenen" Benutzersteuerelemente hinzu.</span><span class="sxs-lookup"><span data-stu-id="b85da-109">Part 7 adds additional features, such as account review, product reviews, and "popular items" and "also purchased" user controls.</span></span>


## <a id="_Toc260221673"></a>  <span data-ttu-id="b85da-110">Hinzufügen von Funktionen</span><span class="sxs-lookup"><span data-stu-id="b85da-110">Adding Features</span></span>

<span data-ttu-id="b85da-111">Obwohl Benutzer unserem Katalog durchsuchen können, platzieren Sie Elemente in ihren Einkaufskorb legen, und abgeschlossen Sie des Auscheckvorgangs, gibt es zahlreiche unterstützende Funktionen, die wir einfügen, um die Verbesserung unserer Website.</span><span class="sxs-lookup"><span data-stu-id="b85da-111">Though users can browse our catalog, place items in their shopping cart, and complete the checkout process, there are a number of supporting features that we will include to improve our site.</span></span>

1. <span data-ttu-id="b85da-112">Überprüfen Sie das Konto (Liste Bestellungen platziert und zeigen Sie Details.)</span><span class="sxs-lookup"><span data-stu-id="b85da-112">Account Review (List orders placed and view details.)</span></span>
2. <span data-ttu-id="b85da-113">Fügen Sie einige spezifischen Kontext-Inhalt, auf die Titelseite.</span><span class="sxs-lookup"><span data-stu-id="b85da-113">Add some context specific content to the front page.</span></span>
3. <span data-ttu-id="b85da-114">Fügen Sie ein Feature, das Benutzern lesen Sie die Produkte im Katalog hinzu.</span><span class="sxs-lookup"><span data-stu-id="b85da-114">Add a feature to let users Review the products in the catalog.</span></span>
4. <span data-ttu-id="b85da-115">Erstellen Sie ein Benutzersteuerelement zum Anzeigen der beliebtesten Elemente und Ort, die steuern, auf der Titelseite ein.</span><span class="sxs-lookup"><span data-stu-id="b85da-115">Create a User Control to display Popular Items and Place that control on the front page.</span></span>
5. <span data-ttu-id="b85da-116">Erstellen eines Benutzersteuerelements "Auch gekauft", und fügen sie die Seite für Produktdetails hinzu.</span><span class="sxs-lookup"><span data-stu-id="b85da-116">Create an "Also Purchased" user control and add it to the product details page.</span></span>
6. <span data-ttu-id="b85da-117">Hinzufügen eines Kontakts Seite.</span><span class="sxs-lookup"><span data-stu-id="b85da-117">Add a Contact Page.</span></span>
7. <span data-ttu-id="b85da-118">Hinzufügen einer zu Seite.</span><span class="sxs-lookup"><span data-stu-id="b85da-118">Add an About Page.</span></span>
8. <span data-ttu-id="b85da-119">Globaler Fehler</span><span class="sxs-lookup"><span data-stu-id="b85da-119">Global Error</span></span>

## <a id="_Toc260221674"></a>  <span data-ttu-id="b85da-120">Konto überprüfen</span><span class="sxs-lookup"><span data-stu-id="b85da-120">Account Review</span></span>

<span data-ttu-id="b85da-121">Erstellen Sie zwei ASPX-Seiten, die eine benannte OrderList.aspx und die andere benannte OrderDetails.aspx im Ordner "Konto"</span><span class="sxs-lookup"><span data-stu-id="b85da-121">In the "Account" folder create two .aspx pages one named OrderList.aspx and the other named OrderDetails.aspx</span></span>

<span data-ttu-id="b85da-122">Ähnlich wie zuvor schon nutzen OrderList.aspx GridView und EntityDataSoure.</span><span class="sxs-lookup"><span data-stu-id="b85da-122">OrderList.aspx will leverage the GridView and EntityDataSoure controls much as we have previously.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample1.aspx)]

<span data-ttu-id="b85da-123">Die EntityDataSoure wählt die Datensätze aus der Tabelle Orders, die den Benutzernamen, das gefiltert (siehe die WhereParameter) die wir in einer Sitzungsvariablen festgelegt, wenn die Benutzeranmeldung in ist.</span><span class="sxs-lookup"><span data-stu-id="b85da-123">The EntityDataSoure selects records from the Orders table filtered on the UserName (see the WhereParameter) which we set in a session variable when the user log's in.</span></span>

<span data-ttu-id="b85da-124">Beachten Sie auch diese Parameter werden in der HyperlinkField der GridView:</span><span class="sxs-lookup"><span data-stu-id="b85da-124">Note also these parameters in the HyperlinkField of the GridView:</span></span>

[!code-xml[Main](tailspin-spyworks-part-7/samples/sample2.xml)]

<span data-ttu-id="b85da-125">Dazu geben Sie den Link zum Anzeigen Details für jedes Produkt, das Feld "OrderID" als QueryString-Parameter auf der Seite "OrderDetails.aspx" angeben.</span><span class="sxs-lookup"><span data-stu-id="b85da-125">These specify the link to the Order details view for each product specifying the OrderID field as a QueryString parameter to the OrderDetails.aspx page.</span></span>

## <a id="_Toc260221675"></a>  <span data-ttu-id="b85da-126">OrderDetails.aspx</span><span class="sxs-lookup"><span data-stu-id="b85da-126">OrderDetails.aspx</span></span>

<span data-ttu-id="b85da-127">Wir verwenden ein EntityDataSource-Steuerelement den Zugriff auf die Bestellungen und einem FormView-Steuerelement zum Anzeigen der Daten und eine andere EntityDataSource mit einer GridView-Ansicht zum Anzeigen aller der Bestellung Einzelposten.</span><span class="sxs-lookup"><span data-stu-id="b85da-127">We will use an EntityDataSource control to access the Orders and a FormView to display the Order data and another EntityDataSource with a GridView to display all the Order's line items.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample3.aspx)]

<span data-ttu-id="b85da-128">In der CodeBehind-Datei (OrderDetails.aspx.cs) haben wir zwei kleine Teile Wartungsaufgaben.</span><span class="sxs-lookup"><span data-stu-id="b85da-128">In the Code Behind file (OrderDetails.aspx.cs) we have two little bits of housekeeping.</span></span>

<span data-ttu-id="b85da-129">Zunächst müssen wir sicherstellen, dass OrderDetails immer ein "OrderID" ruft.</span><span class="sxs-lookup"><span data-stu-id="b85da-129">First we need to make sure that OrderDetails always gets an OrderId.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample4.cs)]

<span data-ttu-id="b85da-130">Wir müssen auch zum Berechnen und die Gesamtsumme aus die Einzelposten des Auftrags anzeigen.</span><span class="sxs-lookup"><span data-stu-id="b85da-130">We also need to calculate and display the order total from the line items.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample5.cs)]

## <a id="_Toc260221676"></a>  <span data-ttu-id="b85da-131">Auf der Startseite</span><span class="sxs-lookup"><span data-stu-id="b85da-131">The Home Page</span></span>

<span data-ttu-id="b85da-132">Fügen Sie einige statische Inhalte auf der Seite "default.aspx" ein.</span><span class="sxs-lookup"><span data-stu-id="b85da-132">Let's add some static content to the Default.aspx page.</span></span>

<span data-ttu-id="b85da-133">Zunächst erstelle ich einen Ordner "Content" und darin einen Ordner "Images" (und ich werde ein Bild auf der Homepage verwendet werden.)</span><span class="sxs-lookup"><span data-stu-id="b85da-133">First I'll create a "Content" folder and within it an Images folder (and I'll include an image to be used on the home page.)</span></span>

<span data-ttu-id="b85da-134">Fügen Sie in den Platzhalter unteren Rand der Seite "default.aspx" das folgende Markup hinzu.</span><span class="sxs-lookup"><span data-stu-id="b85da-134">Into the bottom placeholder of the Default.aspx page, add the following markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample6.aspx)]

## <a id="_Toc260221677"></a>  <span data-ttu-id="b85da-135">Produktbesprechungen</span><span class="sxs-lookup"><span data-stu-id="b85da-135">Product Reviews</span></span>

<span data-ttu-id="b85da-136">Zunächst fügen eine Schaltfläche mit einem Link zu einem Formular wir, die wir verwenden können, die eine produktprüfung eingeben.</span><span class="sxs-lookup"><span data-stu-id="b85da-136">First we'll add a button with a link to a form that we can use to enter a product review.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample7.aspx)]

![](tailspin-spyworks-part-7/_static/image1.jpg)

<span data-ttu-id="b85da-137">Beachten Sie, dass die "ProductID" in der Abfragezeichenfolge übergeben werden</span><span class="sxs-lookup"><span data-stu-id="b85da-137">Note that we are passing the ProductID in the query string</span></span>

<span data-ttu-id="b85da-138">Nächste fügen Sie die Seite mit dem Namen ReviewAdd.aspx</span><span class="sxs-lookup"><span data-stu-id="b85da-138">Next let's add page named ReviewAdd.aspx</span></span>

<span data-ttu-id="b85da-139">Diese Seite wird die ASP.NET AJAX Control Toolkit verwenden.</span><span class="sxs-lookup"><span data-stu-id="b85da-139">This page will use the ASP.NET AJAX Control Toolkit.</span></span> <span data-ttu-id="b85da-140">Wenn Sie noch nicht getan haben, damit Sie es aus herunterladen [DevExpress](http://devexpress.com/act) und Anleitungen zum Einrichten des Toolkits für die Verwendung mit Visual Studio hier [ https://www.asp.net/learn/ajax-videos/video-76.aspx ](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).</span><span class="sxs-lookup"><span data-stu-id="b85da-140">If you have not already done so you can download it from [DevExpress](http://devexpress.com/act) and there is guidance on setting up the toolkit for use with Visual Studio here [https://www.asp.net/learn/ajax-videos/video-76.aspx](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md).</span></span>

<span data-ttu-id="b85da-141">Ziehen Sie im Entwurfsmodus Steuerelemente und Validierungssteuerelemente aus der Toolbox, und erstellen Sie eine Formulierung wie unten angegeben.</span><span class="sxs-lookup"><span data-stu-id="b85da-141">In design mode, drag controls and validators from the toolbox and build a form like the one below.</span></span>

![](tailspin-spyworks-part-7/_static/image2.jpg)

<span data-ttu-id="b85da-142">Das Markup wird wie folgt aussehen.</span><span class="sxs-lookup"><span data-stu-id="b85da-142">The markup will look something like this.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample8.aspx)]

<span data-ttu-id="b85da-143">Nun, da wir Reviews eingeben können, können diese Bewertungen auf der Seite angezeigt werden sollen.</span><span class="sxs-lookup"><span data-stu-id="b85da-143">Now that we can enter reviews, lets display those reviews on the product page.</span></span>

<span data-ttu-id="b85da-144">Fügen Sie dieses Markup auf der Seite "ProductDetails.aspx" hinzu.</span><span class="sxs-lookup"><span data-stu-id="b85da-144">Add this markup to the ProductDetails.aspx page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample9.aspx)]

<span data-ttu-id="b85da-145">Die Anwendung jetzt ausführen, und navigieren Sie zu einem Produkt zeigt die Produktinformationen, einschließlich der Überprüfungen durch den Kunden.</span><span class="sxs-lookup"><span data-stu-id="b85da-145">Running our application now and navigating to a product shows the product information including customer reviews.</span></span>

![](tailspin-spyworks-part-7/_static/image3.jpg)

## <a id="_Toc260221678"></a>  <span data-ttu-id="b85da-146">Beliebte ItemsControl (Erstellen von Benutzersteuerelementen)</span><span class="sxs-lookup"><span data-stu-id="b85da-146">Popular Items Control (Creating User Controls)</span></span>

<span data-ttu-id="b85da-147">Um Umsatzsteigerung auf Ihrer Website fügen wir einige Funktionen auf "vorgeschlagene Sell" gebräuchlichsten und verwandte Produkte hinzu.</span><span class="sxs-lookup"><span data-stu-id="b85da-147">In order to increase sales on your web site we will add a couple of features to "suggestive sell" popular or related products.</span></span>

<span data-ttu-id="b85da-148">Der erste dieser Funktionen wird eine Liste mit den beliebtesten Produkt in unserem Produktkatalog.</span><span class="sxs-lookup"><span data-stu-id="b85da-148">The first of these features will be a list of the more popular product in our product catalog.</span></span>

<span data-ttu-id="b85da-149">Wir erstellen eine "User-Control" um die beliebtesten Elemente auf der Startseite der Anwendung anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="b85da-149">We will create a "User Control" to display the top selling items on the home page of our application.</span></span> <span data-ttu-id="b85da-150">Da dies ein Steuerelement sein wird, können wir es auf einer beliebigen Seite durch einfaches Ziehen und Ablegen des Steuerelements in Visual Studio Designer auf jeder Seite, die uns gefallen.</span><span class="sxs-lookup"><span data-stu-id="b85da-150">Since this will be a control, we can use it on any page by simply dragging and dropping the control in Visual Studio's designer onto any page that we like.</span></span>

<span data-ttu-id="b85da-151">Klicken Sie im Projektmappen-Explorer für Visual Studio mit der rechten Maustaste auf den Namen der Projektmappe, und erstellen Sie ein neues Verzeichnis namens "Steuerelemente".</span><span class="sxs-lookup"><span data-stu-id="b85da-151">In Visual Studio's solutions explorer, right-click on the solution name and create a new directory named "Controls".</span></span> <span data-ttu-id="b85da-152">Obwohl es nicht erforderlich ist, ist, helfen wir unser Projekt, das durch das Erstellen von unserem Benutzersteuerelemente im Verzeichnis "Steuerelemente" zu halten.</span><span class="sxs-lookup"><span data-stu-id="b85da-152">While it is not necessary to do so, we will help keep our project organized by creating all our user controls in the "Controls" directory.</span></span>

<span data-ttu-id="b85da-153">Mit der rechten Maustaste auf den Ordner "Steuerelemente", und wählen Sie "Neues Element":</span><span class="sxs-lookup"><span data-stu-id="b85da-153">Right-click on the controls folder and choose "New Item" :</span></span>

![](tailspin-spyworks-part-7/_static/image4.jpg)

<span data-ttu-id="b85da-154">Geben Sie einen Namen für das Steuerelement von "PopularItems".</span><span class="sxs-lookup"><span data-stu-id="b85da-154">Specify a name for our control of "PopularItems".</span></span> <span data-ttu-id="b85da-155">Beachten Sie, dass die Dateierweiterung für Benutzersteuerelemente ASCX nicht aspx.</span><span class="sxs-lookup"><span data-stu-id="b85da-155">Note that the file extension for user controls is .ascx not .aspx.</span></span>

<span data-ttu-id="b85da-156">Unsere beliebten Elemente Benutzersteuerelement wird wie folgt definiert werden.</span><span class="sxs-lookup"><span data-stu-id="b85da-156">Our Popular Items User control will be defined as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample10.aspx)]

<span data-ttu-id="b85da-157">Hier verwenden wir eine Methode, die wir noch nicht in dieser Anwendung verwendet haben.</span><span class="sxs-lookup"><span data-stu-id="b85da-157">Here we're using a method we have not used yet in this application.</span></span> <span data-ttu-id="b85da-158">Wir verwenden das Repeater-Steuerelement, und anstelle von Datenquellen-Steuerelement sind wir das Repeater-Steuerelement binden, um die Ergebnisse einer LINQ to Entities-Abfrage.</span><span class="sxs-lookup"><span data-stu-id="b85da-158">We're using the repeater control and instead of using a data source control we're binding the Repeater Control to the results of a LINQ to Entities query.</span></span>

<span data-ttu-id="b85da-159">In den Code hinter der das Steuerelement das machen wir wie folgt.</span><span class="sxs-lookup"><span data-stu-id="b85da-159">In the code behind of our control we do that as follows.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample11.cs)]

<span data-ttu-id="b85da-160">Beachten Sie auch diese wichtige Zeile am Anfang Markup des Steuerelements.</span><span class="sxs-lookup"><span data-stu-id="b85da-160">Note also this important line at the top of our control's markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample12.aspx)]

<span data-ttu-id="b85da-161">Da die am häufigsten verwendeten Elemente pro minütlich nicht ändert, können wir eine schmerzenden Direktive zur Verbesserung der Leistung der Anwendung hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="b85da-161">Since the most popular items won't be changing on a minute to minute basis we can add a aching directive to improve the performance of our application.</span></span> <span data-ttu-id="b85da-162">Diese Anweisung bewirkt, dass die Steuerelemente Code nur ausgeführt werden, wenn die zwischengespeicherte Ausgabe des Steuerelements abläuft.</span><span class="sxs-lookup"><span data-stu-id="b85da-162">This directive will cause the controls code to only be executed when the cached output of the control expires.</span></span> <span data-ttu-id="b85da-163">Andernfalls wird die zwischengespeicherte Version der die Ausgabe des Steuerelements verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="b85da-163">Otherwise, the cached version of the control's output will be used.</span></span>

<span data-ttu-id="b85da-164">Jetzt alles, was schon alles ist das neue Steuerelement auf unserer Seite Default.aspc enthalten.</span><span class="sxs-lookup"><span data-stu-id="b85da-164">Now all we have to do is include our new control in our Default.aspc page.</span></span>

<span data-ttu-id="b85da-165">Mithilfe von ziehen und ablegen, um eine Instanz des Steuerelements in der Spalte öffnen das Standard-Formular zu platzieren.</span><span class="sxs-lookup"><span data-stu-id="b85da-165">Use drag and drop to place an instance of the control in the open column of our Default form.</span></span>

![](tailspin-spyworks-part-7/_static/image5.jpg)

<span data-ttu-id="b85da-166">Wenn wir unsere Anwendung auf der Startseite ausführen zeigt die am häufigsten verwendeten Elemente jetzt.</span><span class="sxs-lookup"><span data-stu-id="b85da-166">Now when we run our application the home page displays the most popular items.</span></span>

![](tailspin-spyworks-part-7/_static/image6.jpg)

## <a id="_Toc260221679"></a>  <span data-ttu-id="b85da-167">"Auch gekauft" zu steuern (Benutzersteuerelemente mit Parametern)</span><span class="sxs-lookup"><span data-stu-id="b85da-167">"Also Purchased" Control (User Controls with Parameters)</span></span>

<span data-ttu-id="b85da-168">Das zweite Steuerelement, das wir erstellen dauert vorgeschlagenen auf die nächste Stufe durch Hinzufügen von Kontext Spezifität verkaufen.</span><span class="sxs-lookup"><span data-stu-id="b85da-168">The second User Control that we'll create will take suggestive selling to the next level by adding context specificity.</span></span>

<span data-ttu-id="b85da-169">Die Logik zum Berechnen Sie die Elemente "Auch gekauft" ist nicht trivial.</span><span class="sxs-lookup"><span data-stu-id="b85da-169">The logic to calculate the top "Also Purchased" items is non-trivial.</span></span>

<span data-ttu-id="b85da-170">Unsere "Auch gekauft"-Steuerelement die OrderDetails-Datensätze, die (zuvor gekauft) für den aktuell ausgewählten "ProductID" auswählen und ziehen Sie die Auftrags-ID für jede eindeutige Bestellung, die gefunden wurde.</span><span class="sxs-lookup"><span data-stu-id="b85da-170">Our "Also Purchased" control will select the OrderDetails records (previously purchased) for the currently selected ProductID and grab the OrderIDs for each unique order that is found.</span></span>

<span data-ttu-id="b85da-171">Klicken Sie dann werden wir al wählen Sie die Produkte aus allen Bestellungen und Sum Mengen erworben haben.</span><span class="sxs-lookup"><span data-stu-id="b85da-171">Then we will select al the products from all those Orders and sum the quantities purchased.</span></span> <span data-ttu-id="b85da-172">Wir sortieren die Produkte, indem die Summe der Menge und zeigt die ersten fünf Elemente.</span><span class="sxs-lookup"><span data-stu-id="b85da-172">We'll sort the products by that quantity sum and display the top five items.</span></span>

<span data-ttu-id="b85da-173">Angesichts die Komplexität dieser Logik, werden wir diesen Algorithmus als eine gespeicherte Prozedur implementieren.</span><span class="sxs-lookup"><span data-stu-id="b85da-173">Given the complexity of this logic, we will implement this algorithm as a stored procedure.</span></span>

<span data-ttu-id="b85da-174">Das T-SQL für die gespeicherte Prozedur lautet wie folgt aus:</span><span class="sxs-lookup"><span data-stu-id="b85da-174">The T-SQL for the stored procedure is as follows.</span></span>

[!code-sql[Main](tailspin-spyworks-part-7/samples/sample13.sql)]

<span data-ttu-id="b85da-175">Beachten Sie, dass diese gespeicherte Prozedur (SelectPurchasedWithProducts), die in der Datenbank vorhanden waren, als wir haben es in unserer Anwendung, und wenn wir, dass zusätzlich zu den Tabellen und Sichten, die es erforderlich, das Entity Data Model angegebenen Entity Data Model generierten eingefügt Diese gespeicherte Prozedur sollte enthalten werden.</span><span class="sxs-lookup"><span data-stu-id="b85da-175">Note that this stored procedure (SelectPurchasedWithProducts) existed in the database when we included it in our application and when we generated the Entity Data Model we specified that, in addition to the Tables and Views that we needed, the Entity Data Model should include this stored procedure.</span></span>

<span data-ttu-id="b85da-176">Die gespeicherte Prozedur aus dem Entity Data Model für den Zugriff auf müssen wir die Funktion zu importieren.</span><span class="sxs-lookup"><span data-stu-id="b85da-176">To access the stored procedure from the Entity Data Model we need to import the function.</span></span>

<span data-ttu-id="b85da-177">Einen Doppelklick auf das Entity Data Model im Projektmappen-Explorer im Designer zu öffnen, und öffnen den Browser, und klicken Sie dann mit der rechten Maustaste im Designer, und wählen Sie "Funktionsimport hinzufügen".</span><span class="sxs-lookup"><span data-stu-id="b85da-177">Double Click on the Entity Data Model in the Solutions Explorer to open it in the designer and open the Model Browser, then right-click in the designer and select "Add Function Import".</span></span>

![](tailspin-spyworks-part-7/_static/image1.png)

<span data-ttu-id="b85da-178">Auf diese Weise wird dieses Dialogfeld geöffnet.</span><span class="sxs-lookup"><span data-stu-id="b85da-178">Doing so will open this dialog.</span></span>

![](tailspin-spyworks-part-7/_static/image2.png)

<span data-ttu-id="b85da-179">Füllen Sie die Felder, wie Sie oben sehen, die "SelectPurchasedWithProducts" auswählen, und verwenden Sie den Namen der Prozedur für den Namen unserer importierten Funktion.</span><span class="sxs-lookup"><span data-stu-id="b85da-179">Fill out the fields as you see above, selecting the "SelectPurchasedWithProducts" and use the procedure name for the name of our imported function.</span></span>

<span data-ttu-id="b85da-180">Klicken Sie auf "Ok".</span><span class="sxs-lookup"><span data-stu-id="b85da-180">Click "Ok".</span></span>

<span data-ttu-id="b85da-181">Danach, die wir einfach die gespeicherte Prozedur programmieren können, wie wir jedes andere Element im Modell.</span><span class="sxs-lookup"><span data-stu-id="b85da-181">Having done this we can simply program against the stored procedure as we might any other item in the model.</span></span>

<span data-ttu-id="b85da-182">Erstellen Sie ein neues Benutzersteuerelement mit dem Namen AlsoPurchased.ascx also in den Ordner "Steuerelemente".</span><span class="sxs-lookup"><span data-stu-id="b85da-182">So, in our "Controls" folder create a new user control named AlsoPurchased.ascx.</span></span>

<span data-ttu-id="b85da-183">Das Markup für dieses Steuerelement wird an das Steuerelement PopularItems vertraut sein.</span><span class="sxs-lookup"><span data-stu-id="b85da-183">The markup for this control will look very familiar to the PopularItems control.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample14.aspx)]

<span data-ttu-id="b85da-184">Der wichtige Unterschied ist, dass, die nicht die Zwischenspeicherung der Ausgabe werden, da des Elements, das gerendert werden vom Produkt voneinander unterscheiden.</span><span class="sxs-lookup"><span data-stu-id="b85da-184">The notable difference is that are not caching the output since the item's to be rendered will differ by product.</span></span>

<span data-ttu-id="b85da-185">Die "ProductID" wird eine "Property" für das Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="b85da-185">The ProductId will be a "property" to the control.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample15.cs)]

<span data-ttu-id="b85da-186">In PreRender Ereignishandler des Steuerelements wir Eed drei Dinge erreichen.</span><span class="sxs-lookup"><span data-stu-id="b85da-186">In the control's PreRender event handler we eed to do three things.</span></span>

1. <span data-ttu-id="b85da-187">Stellen Sie sicher, dass die "ProductID" festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="b85da-187">Make sure the ProductID is set.</span></span>
2. <span data-ttu-id="b85da-188">Überprüfen Sie, ob alle Produkte, die zuvor erworben wurden, mit der aktuellen Instanz.</span><span class="sxs-lookup"><span data-stu-id="b85da-188">See if there are any products that have been purchased with the current one.</span></span>
3. <span data-ttu-id="b85da-189">Ausgabe einige Elemente wie #2 festgelegt.</span><span class="sxs-lookup"><span data-stu-id="b85da-189">Output some items as determined in #2.</span></span>

<span data-ttu-id="b85da-190">Beachten Sie, wie einfach es ist, rufen Sie die gespeicherte Prozedur über das Modell.</span><span class="sxs-lookup"><span data-stu-id="b85da-190">Note how easy it is to call the stored procedure through the model.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample16.cs)]

<span data-ttu-id="b85da-191">Nachdem ermittelt wurde, gibt es "auch erworben werden" können wir einfach die Repeater, die von der Abfrage zurückgegebenen Ergebnisse binden.</span><span class="sxs-lookup"><span data-stu-id="b85da-191">After determining that there ARE "also purchased" we can simply bind the repeater to the results returned by the query.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample17.cs)]

<span data-ttu-id="b85da-192">Gäbe es keine Elemente "auch gekauft" zeigen wir einfach andere beliebte Elemente aus unserem Katalog.</span><span class="sxs-lookup"><span data-stu-id="b85da-192">If there were not any "also purchased" items we'll simply display other popular items from our catalog.</span></span>

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample18.cs)]

<span data-ttu-id="b85da-193">Die "Auch gekauft" Elemente anzeigen, öffnen Sie die Seite ProductDetails.aspx, und ziehen das AlsoPurchased-Steuerelement aus dem Projektmappen-Explorer, sodass sie an dieser Position im Markup angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="b85da-193">To view the "Also Purchased" items, open the ProductDetails.aspx page and drag the AlsoPurchased control from the Solutions Explorer so that it appears in this position in the markup.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample19.aspx)]

<span data-ttu-id="b85da-194">Auf diese Weise wird einen Verweis auf das Steuerelement am oberen Rand der Seite "ProductDetails" erstellt.</span><span class="sxs-lookup"><span data-stu-id="b85da-194">Doing so will create a reference to the control at the top of the ProductDetails page.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample20.aspx)]

<span data-ttu-id="b85da-195">Da das Benutzersteuerelement AlsoPurchased eine Reihe von "ProductID" erfordert werden wir die ProductID-Eigenschaft, der das Steuerelement mit einer Eval-Anweisung für das aktuelle Modell-Datenelement der Seite festgelegt.</span><span class="sxs-lookup"><span data-stu-id="b85da-195">Since the AlsoPurchased user control requires a ProductId number we will set the ProductID property of our control by using an Eval statement against the current data model item of the page.</span></span>

![](tailspin-spyworks-part-7/_static/image3.png)

<span data-ttu-id="b85da-196">Wenn wir erstellen, und jetzt ausführen, und navigieren Sie zu einem Produkt sehen Sie die Elemente "Auch gekauft".</span><span class="sxs-lookup"><span data-stu-id="b85da-196">When we build and run now and browse to a product we see the "Also Purchased" items.</span></span>

![](tailspin-spyworks-part-7/_static/image7.jpg)

> [!div class="step-by-step"]
> <span data-ttu-id="b85da-197">[Zurück](tailspin-spyworks-part-6.md)
> [Weiter](tailspin-spyworks-part-8.md)</span><span class="sxs-lookup"><span data-stu-id="b85da-197">[Previous](tailspin-spyworks-part-6.md)
[Next](tailspin-spyworks-part-8.md)</span></span>
