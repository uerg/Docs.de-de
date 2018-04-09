---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
title: 'Teil 4: Auflisten von Produkten | Microsoft Docs'
author: JoeStagner
description: Diese Reihe von Lernprogrammen sind alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen. Teil 4 deckt Auflisten von Produkten mit der GridView Vertr....
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 4fab47d5-a6ec-4fdc-91f0-651a093a24b9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
msc.type: authoredcontent
ms.openlocfilehash: 69b26344e6dcdbf27e94da90ad5d6cd79f27ccd3
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="part-4-listing-products"></a><span data-ttu-id="d2289-104">Teil 4: Angebot Produkte</span><span class="sxs-lookup"><span data-stu-id="d2289-104">Part 4: Listing Products</span></span>
====================
<span data-ttu-id="d2289-105">durch [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="d2289-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="d2289-106">Tailspin Spyworks wird veranschaulicht, wie außergewöhnlich einfache ist die leistungsstarke, skalierbare Anwendungen für .NET-Plattform zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="d2289-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="d2289-107">Es wird gezeigt, aus wie die hervorragenden neuen Funktionen in ASP.NET 4 mit um einen Onlineshop, einschließlich Warenkorb, Auschecken und Verwaltung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="d2289-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="d2289-108">Diese Reihe von Lernprogrammen sind alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen.</span><span class="sxs-lookup"><span data-stu-id="d2289-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="d2289-109">Teil 4 werden die Produkte Angebot des GridView-Steuerelements behandelt.</span><span class="sxs-lookup"><span data-stu-id="d2289-109">Part 4 covers listing products with the GridView control.</span></span>


## <a id="_Toc260221670"></a>  <span data-ttu-id="d2289-110">Auflisten von Produkten mit des GridView-Steuerelements</span><span class="sxs-lookup"><span data-stu-id="d2289-110">Listing Products with the GridView Control</span></span>

<span data-ttu-id="d2289-111">Fangen wir implementieren unsere ProductsList.aspx-Seite "Mit der rechten Maustaste auf" auf unserer Projektmappe, und wählen "Hinzufügen" und "Neues Element".</span><span class="sxs-lookup"><span data-stu-id="d2289-111">Let's begin implementing our ProductsList.aspx page by "Right Clicking" on our solution and selecting "Add" and "New Item".</span></span>

![](tailspin-spyworks-part-4/_static/image1.jpg)

<span data-ttu-id="d2289-112">Wählen Sie "Web Form mithilfe Masterseite", und geben Sie einen Seitennamen ProductsList.aspx".</span><span class="sxs-lookup"><span data-stu-id="d2289-112">Choose "Web Form Using Master Page" and enter a page name of ProductsList.aspx".</span></span>

<span data-ttu-id="d2289-113">Klicken Sie auf "Hinzufügen".</span><span class="sxs-lookup"><span data-stu-id="d2289-113">Click "Add".</span></span>

![](tailspin-spyworks-part-4/_static/image2.jpg)

<span data-ttu-id="d2289-114">Als nächstes wählen Sie den Ordner "Formatvorlagen", in dem wir die Site.Master-Seite platziert, und wählen Sie ihn aus dem Fenster "Inhalt des Ordners".</span><span class="sxs-lookup"><span data-stu-id="d2289-114">Next choose the "Styles" folder where we placed the Site.Master page and select it from the "Contents of folder" window.</span></span>

![](tailspin-spyworks-part-4/_static/image3.jpg)

<span data-ttu-id="d2289-115">Klicken Sie auf "Ok", um die Seite zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="d2289-115">Click "Ok" to create the page.</span></span>

<span data-ttu-id="d2289-116">Die Datenbank wird mit Produktdaten aufgefüllt, wie unten dargestellt.</span><span class="sxs-lookup"><span data-stu-id="d2289-116">Our database is populated with product data as seen below.</span></span>

![](tailspin-spyworks-part-4/_static/image4.jpg)

<span data-ttu-id="d2289-117">Nach der Erstellung unserer Seite wir eine Datenquelle für die Entität erneut Zugriff auf diese Produktdaten verwenden, aber in diesem Fall müssen wir die Product-Entitäten auswählen, und wir müssen Sie die Elemente begrenzen, die zurückgegeben werden, um nur die für die ausgewählte Kategorie.</span><span class="sxs-lookup"><span data-stu-id="d2289-117">After our page is created we'll again use an Entity Data Source to access that product data, but in this instance we need to select the Product Entities and we need to restrict the items that are returned to only those for the selected Category.</span></span>

<span data-ttu-id="d2289-118">Um dies zu erreichen erfahren EntityDataSource zum automatischen Generieren der WHERE-Klausel aus, und geben wir die WhereParameter.</span><span class="sxs-lookup"><span data-stu-id="d2289-118">To accomplish this we'll tell the EntityDataSource to Auto Generate the WHERE clause and we'll specify the WhereParameter.</span></span>

<span data-ttu-id="d2289-119">Beachten Sie, dass bei der Menüelemente in unserem "Product Category-Menü" erstellt haben wir dynamisch den Link erstellt die Abfragezeichenfolge für jeden Link der CatagoryID hinzugefügt wird.</span><span class="sxs-lookup"><span data-stu-id="d2289-119">You'll recall that when we created the Menu Items in our "Product Category Menu" we dynamically built the link by adding the CatagoryID to the QueryString for each link.</span></span> <span data-ttu-id="d2289-120">Wir werden diesen QueryString-Parameter den WHERE-Parameter Ableiten der Entität-Datenquelle informieren.</span><span class="sxs-lookup"><span data-stu-id="d2289-120">We will tell the Entity Data Source to derive the WHERE parameter from that QueryString parameter.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample1.aspx)]

<span data-ttu-id="d2289-121">Konfigurieren Sie anschließend das ListView-Steuerelement, um eine Liste von Produkten anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="d2289-121">Next, we'll configure the ListView control to display a list of products.</span></span> <span data-ttu-id="d2289-122">Um eine optimale Leistung Warenkorb erstellen wir mehrere präzise Funktionen in jedes einzelne Produkt angezeigt, in unserem ListVew komprimiert werden.</span><span class="sxs-lookup"><span data-stu-id="d2289-122">To create an optimal shopping experience we'll compact several concise features into each individual product displayed in our ListVew.</span></span>

- <span data-ttu-id="d2289-123">Der Produktname wird ein Link auf der Produkt-Detailansicht sein.</span><span class="sxs-lookup"><span data-stu-id="d2289-123">The product name will be a link to the product's detail view.</span></span>
- <span data-ttu-id="d2289-124">Das Produkt Preis wird angezeigt.</span><span class="sxs-lookup"><span data-stu-id="d2289-124">The product's price will be displayed.</span></span>
- <span data-ttu-id="d2289-125">Ein Bild des Produkts angezeigt, und wir dynamisch wähle das Image aus einem Katalog-Images-Verzeichnis in der vorliegenden Anwendung.</span><span class="sxs-lookup"><span data-stu-id="d2289-125">An image of the product will be displayed and we'll dynamically select the image from a catalog images directory in our application.</span></span>
- <span data-ttu-id="d2289-126">Es enthält einen Link, um sofort des bestimmten Produkts zum Einkaufswagen hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="d2289-126">We will include a link to immediately add the specific product to the shopping cart.</span></span>

<span data-ttu-id="d2289-127">Im folgenden wird das Markup für die gegebene Instanz des ListView-Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="d2289-127">Here is the markup for our ListView control instance.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample2.aspx)]

<span data-ttu-id="d2289-128">Dynamisch erstellen wir einige Links für die einzelnen Produkte angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="d2289-128">We are dynamically building several links for each displayed product.</span></span>

<span data-ttu-id="d2289-129">Bevor wir eigenen neuen Seite testen müssen wir darüber hinaus die Verzeichnisstruktur für das Produkt Katalog-Images erstellen, die wie folgt.</span><span class="sxs-lookup"><span data-stu-id="d2289-129">Also, before we test own new page we need to create the directory structure for the product catalog images as follows.</span></span>

![](tailspin-spyworks-part-4/_static/image1.png)

<span data-ttu-id="d2289-130">Nach unserer Produktbilder zugegriffen werden können wir unser Produkt Listenseite testen.</span><span class="sxs-lookup"><span data-stu-id="d2289-130">Once our product images are accessible we can test our product list page.</span></span>

![](tailspin-spyworks-part-4/_static/image5.jpg)

<span data-ttu-id="d2289-131">Homepage der Website klicken Sie auf einen der Links Liste Kategorie.</span><span class="sxs-lookup"><span data-stu-id="d2289-131">From the site's home page, click on one of the Category List Links.</span></span>

![](tailspin-spyworks-part-4/_static/image6.jpg)

<span data-ttu-id="d2289-132">Nun müssen wir die Seite "ProductDetials.apsx" und die AddToCart-Funktionalität zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="d2289-132">Now we need to implement the ProductDetials.apsx page and the AddToCart functionality.</span></span>

<span data-ttu-id="d2289-133">Mithilfe des Datei -&gt;neu erstellen Sie einen Seitennamen ProductDetails.aspx über Website für die Gestaltungsvorlage aus, wie zuvor.</span><span class="sxs-lookup"><span data-stu-id="d2289-133">Use File-&gt;New to create a page name ProductDetails.aspx using the site Master Page as we did previously.</span></span>

<span data-ttu-id="d2289-134">Wir verwenden erneut EntityDataSource-Steuerelement, um den bestimmten Produktdatensatz in der Datenbank zugreifen, und verwenden wir ein ASP.NET FormView-Steuerelement zum Anzeigen von der Produktdaten wie folgt.</span><span class="sxs-lookup"><span data-stu-id="d2289-134">We will again use an EntityDataSource control to access the specific product record in the database and we will use an ASP.NET FormView control to display the product data as follows.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample3.aspx)]

<span data-ttu-id="d2289-135">Machen Sie sich keine Gedanken Sie, wenn die Formatierung für Sie ein wenig seltsam aussieht.</span><span class="sxs-lookup"><span data-stu-id="d2289-135">Don't worry if the formatting looks a bit funny to you.</span></span> <span data-ttu-id="d2289-136">Das Markup oben verlässt Platz im Anzeigelayout für eine Reihe von Funktionen, die wir später implementieren.</span><span class="sxs-lookup"><span data-stu-id="d2289-136">The markup above leaves room in the display layout for a couple of features we'll implement later on.</span></span>

<span data-ttu-id="d2289-137">Der Einkaufswagen repräsentiert die komplexere Logik in der vorliegenden Anwendung.</span><span class="sxs-lookup"><span data-stu-id="d2289-137">The Shopping Cart will represent the more complex logic in our application.</span></span> <span data-ttu-id="d2289-138">Um zu beginnen, mithilfe des Datei -&gt;neu zum Erstellen eines Seitenblob MyShoppingCart.aspx aufgerufen.</span><span class="sxs-lookup"><span data-stu-id="d2289-138">To get started, use File-&gt;New to create a page called MyShoppingCart.aspx.</span></span>

<span data-ttu-id="d2289-139">Beachten Sie, dass wir den Namen ShoppingCart.aspx nicht auswählen.</span><span class="sxs-lookup"><span data-stu-id="d2289-139">Note that we are not choosing the name ShoppingCart.aspx.</span></span>

<span data-ttu-id="d2289-140">Die Datenbank enthält eine Tabelle mit dem Namen "ShoppingCart".</span><span class="sxs-lookup"><span data-stu-id="d2289-140">Our database contains a table named "ShoppingCart".</span></span> <span data-ttu-id="d2289-141">Wenn wir ein Entity Data Model generiert wurde eine Klasse für jede Tabelle in der Datenbank erstellt.</span><span class="sxs-lookup"><span data-stu-id="d2289-141">When we generated an Entity Data Model a class was created for each table in the database.</span></span> <span data-ttu-id="d2289-142">Das Entity Data Model generiert daher eine Entitätsklasse, die mit dem Namen "ShoppingCart".</span><span class="sxs-lookup"><span data-stu-id="d2289-142">Therefore, the Entity Data Model generated an Entity Class named "ShoppingCart".</span></span> <span data-ttu-id="d2289-143">Wir konnten das Modell bearbeiten, sodass konnten wir verwenden Sie diesen Namen für unsere shopping Cart-Implementierung oder für unseren Anforderungen zu erweitern, aber wir stattdessen, wählen Sie einfach einen Namen, der den Konflikt zu vermeiden, wird deaktiviert wird.</span><span class="sxs-lookup"><span data-stu-id="d2289-143">We could edit the model so that we could use that name for our shopping cart implementation or extend it for our needs, but we will opt instead to simply slect a name that will avoid the conflict.</span></span>

<span data-ttu-id="d2289-144">Es ist auch Folgendes zu beachten, dass wir einen einfache Einkaufswagen erstellen und die shopping Cart-Logik mit shopping Cart-Anzeige einbetten.</span><span class="sxs-lookup"><span data-stu-id="d2289-144">It's also worth noting that we will be creating a simple shopping cart and embedding the shopping cart logic with the shopping cart display.</span></span> <span data-ttu-id="d2289-145">Es empfiehlt sich auch unsere warenkorbsoftware in einer vollständig separaten Business-Ebene zu implementieren.</span><span class="sxs-lookup"><span data-stu-id="d2289-145">We might also choose to implement our shopping cart in a completely separate Business Layer.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d2289-146">[Zurück](tailspin-spyworks-part-3.md)
> [Weiter](tailspin-spyworks-part-5.md)</span><span class="sxs-lookup"><span data-stu-id="d2289-146">[Previous](tailspin-spyworks-part-3.md)
[Next](tailspin-spyworks-part-5.md)</span></span>
