---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
title: "Teil 3: Layout und die Kategorie Menü | Microsoft Docs"
author: JoeStagner
description: "Diese Reihe von Lernprogrammen sind alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen. Teil 3 beschreibt das Hinzufügen von Layout und ein Kategorie-Menü."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 94ea1a70-a9bc-4241-8f36-08366d64bab9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
msc.type: authoredcontent
ms.openlocfilehash: 57c0342efb67b94a0d8c8b06dc13a727e7184db8
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="part-3-layout-and-category-menu"></a><span data-ttu-id="fedf1-104">Teil 3: Layout und die Kategorie Menü</span><span class="sxs-lookup"><span data-stu-id="fedf1-104">Part 3: Layout and Category Menu</span></span>
====================
<span data-ttu-id="fedf1-105">durch [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="fedf1-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="fedf1-106">Tailspin Spyworks wird veranschaulicht, wie außergewöhnlich einfache ist die leistungsstarke, skalierbare Anwendungen für .NET-Plattform zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="fedf1-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="fedf1-107">Es wird gezeigt, aus wie die hervorragenden neuen Funktionen in ASP.NET 4 mit um einen Onlineshop, einschließlich Warenkorb, Auschecken und Verwaltung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="fedf1-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="fedf1-108">Diese Reihe von Lernprogrammen sind alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen.</span><span class="sxs-lookup"><span data-stu-id="fedf1-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="fedf1-109">Teil 3 beschreibt das Hinzufügen von Layout und ein Kategorie-Menü.</span><span class="sxs-lookup"><span data-stu-id="fedf1-109">Part 3 covers adding layout and a category menu.</span></span>


## <a id="_Toc260221669"></a><span data-ttu-id="fedf1-110">Einige Layout und ein Menü Kategorie hinzufügen</span><span class="sxs-lookup"><span data-stu-id="fedf1-110">Adding Some Layout and a Category Menu</span></span>

<span data-ttu-id="fedf1-111">Auf unserer Website Masterseite fügen wir ein div-Tag für die linke Spalte, die unsere Product Category-Menü enthält.</span><span class="sxs-lookup"><span data-stu-id="fedf1-111">In our site master page we'll add a div for the left side column that will contain our product category menu.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample1.aspx)]

<span data-ttu-id="fedf1-112">Beachten Sie, dass die gewünschte Ausrichtung und andere Formatierung von CSS-Klasse bereitgestellt werden, die wir unsere Datei "Style.css" hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="fedf1-112">Note that the desired aligning and other formatting will be provided by the CSS class that we added to our Style.css file.</span></span>

[!code-css[Main](tailspin-spyworks-part-3/samples/sample2.css)]

<span data-ttu-id="fedf1-113">Das Product Category-Menü wird dynamisch erstellt zur Laufzeit durch Abfragen der Commerce-Datenbank, für die vorhandene Produktkategorien und erstellen die Menüelemente und entsprechenden verknüpft.</span><span class="sxs-lookup"><span data-stu-id="fedf1-113">The product category menu will be dynamically created at runtime by querying the Commerce database for existing product categories and creating the menu items and corresponding links.</span></span>

<span data-ttu-id="fedf1-114">Zu diesem Zweck verwenden wir zwei von ASP. NET leistungsstarke Data-Steuerelemente.</span><span class="sxs-lookup"><span data-stu-id="fedf1-114">To accomplish this we will use two of ASP.NET's powerful data controls.</span></span> <span data-ttu-id="fedf1-115">Das Steuerelement "Entity Data Source" und "ListView"-Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="fedf1-115">The "Entity Data Source" control and the "ListView" control.</span></span>

![](tailspin-spyworks-part-3/_static/image1.jpg)

<span data-ttu-id="fedf1-116">Wir wechseln Sie zu "Entwurfsansicht", und verwenden Sie die Hilfsprogramme, um unsere Steuerelemente zu konfigurieren.</span><span class="sxs-lookup"><span data-stu-id="fedf1-116">Let's switch to "Design View" and use the helpers to configure our controls.</span></span>

![](tailspin-spyworks-part-3/_static/image2.jpg)

<span data-ttu-id="fedf1-117">Legen Sie die EntityDataSource-ID-Eigenschaft für EDS\_Kategorie\_Menü, und klicken Sie auf "Datenquelle konfigurieren".</span><span class="sxs-lookup"><span data-stu-id="fedf1-117">Let's set the EntityDataSource ID property to EDS\_Category\_Menu and click on "Configure Data Source".</span></span>

![](tailspin-spyworks-part-3/_static/image3.jpg)

<span data-ttu-id="fedf1-118">Wählen Sie die CommerceEntities-Verbindung, die für uns erstellt wurde, wenn wir die Quelle des Entitätsdatenmodells für unsere Commerce-Datenbank erstellt, und klicken Sie auf "Weiter".</span><span class="sxs-lookup"><span data-stu-id="fedf1-118">Select the CommerceEntities Connection that was created for us when we created the Entity Data Source Model for our Commerce Database and click "Next".</span></span>

![](tailspin-spyworks-part-3/_static/image4.jpg)

<span data-ttu-id="fedf1-119">Wählen Sie den Namen der Entitätenmenge "Kategorien", und übernehmen Sie die übrigen Optionen als Standard.</span><span class="sxs-lookup"><span data-stu-id="fedf1-119">Select the "Categories" Entity set name and leave the rest of the options as default.</span></span> <span data-ttu-id="fedf1-120">Klicken Sie auf "Fertig stellen".</span><span class="sxs-lookup"><span data-stu-id="fedf1-120">Click "Finish".</span></span>

<span data-ttu-id="fedf1-121">Nun legen Sie die ID-Eigenschaft der ListView-Steuerelement-Instanz, die wir auf unserer Seite ListView platziert\_ProductsMenu, und aktivieren Sie die Hilfsfunktion.</span><span class="sxs-lookup"><span data-stu-id="fedf1-121">Now let's set the ID property of the ListView control instance that we placed on our page to ListView\_ProductsMenu and activate its helper.</span></span>

![](tailspin-spyworks-part-3/_static/image5.jpg)

<span data-ttu-id="fedf1-122">Obwohl wir konnte Steuerelementoptionen verwenden, um die Anzeige der Daten zu formatieren und Formatierung, unsere im Menü erstellen wird nur erfordern einfache Markup damit wir den Code in der Quellansicht eingeben, wird.</span><span class="sxs-lookup"><span data-stu-id="fedf1-122">Though we could use control options to format the data item display and formatting, our menu creation will only require simple markup so we will enter the code in the source view.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample3.aspx)]

<span data-ttu-id="fedf1-123">Bitte beachten Sie die Anweisung "Eval": &lt;% # Eval("CategoryName") %&gt;</span><span class="sxs-lookup"><span data-stu-id="fedf1-123">Please note the "Eval" statement : &lt;%# Eval("CategoryName") %&gt;</span></span>

<span data-ttu-id="fedf1-124">Der ASP.NET-Syntax &lt;% # %&gt; ist eine Kurzschreibweise-Konvention, die weist das Laufzeitmodul an, wie in enthalten ist, und gibt die Ergebnisse aus "in Zeile" ausführen.</span><span class="sxs-lookup"><span data-stu-id="fedf1-124">The ASP.NET syntax &lt;%# %&gt; is a shorthand convention that instructs the runtime to execute whatever is contained within and output the results "in Line".</span></span>

<span data-ttu-id="fedf1-125">Die Anweisung Eval("CategoryName") angewiesen, die für den aktuellen Eintrag in der gebundenen Auflistung von Datenelementen, rufen den Wert des Entitätsmodell-Elementnamen "CatagoryName" ab.</span><span class="sxs-lookup"><span data-stu-id="fedf1-125">The statement Eval("CategoryName") instructs that, for the current entry in the bound collection of data items, fetch the value of the Entity Model item names "CatagoryName".</span></span> <span data-ttu-id="fedf1-126">Dies ist die kurze Syntax für eine sehr leistungsstarke Funktion.</span><span class="sxs-lookup"><span data-stu-id="fedf1-126">This is concise syntax for a very powerful feature.</span></span>

<span data-ttu-id="fedf1-127">Können die Anwendung jetzt ausführen.</span><span class="sxs-lookup"><span data-stu-id="fedf1-127">Lets run the application now.</span></span>

![](tailspin-spyworks-part-3/_static/image6.jpg)

<span data-ttu-id="fedf1-128">Beachten Sie, dass unsere Product Category-Menü jetzt angezeigt wird. wenn es für eine Kategorie Menüelemente zeigen sehen wir die im Menü Element-Link verweist auf eine Seite, die wir noch implementieren müssen mit dem Namen ProductsList.aspx und enthält es ein Zeichenfolgenargument dynamischen Abfrage erstellt haben die  Id der Kategorie.</span><span class="sxs-lookup"><span data-stu-id="fedf1-128">Note that our product category menu is now displayed and when we hover over one of the category menu items we can see the menu item link points to a page we have yet to implement named ProductsList.aspx and that we have built a dynamic query string argument that contains the category id.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="fedf1-129">[Zurück](tailspin-spyworks-part-2.md)
[Weiter](tailspin-spyworks-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="fedf1-129">[Previous](tailspin-spyworks-part-2.md)
[Next](tailspin-spyworks-part-4.md)</span></span>
