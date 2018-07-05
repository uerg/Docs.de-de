---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
title: 'Teil 3: Layout- und Kategoriemenü | Microsoft-Dokumentation'
author: JoeStagner
description: Dieser tutorialreihe werden alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen. Teil 3 beschreibt das Hinzufügen von Layout und ein kategoriemenü.
ms.author: aspnetcontent
ms.date: 07/21/2010
ms.assetid: 94ea1a70-a9bc-4241-8f36-08366d64bab9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-3
msc.type: authoredcontent
ms.openlocfilehash: 3047c53e21a418aef8617bd772a247bc46edb98f
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817524"
---
<a name="part-3-layout-and-category-menu"></a><span data-ttu-id="3574e-104">Teil 3: Layout- und Kategoriemenü</span><span class="sxs-lookup"><span data-stu-id="3574e-104">Part 3: Layout and Category Menu</span></span>
====================
<span data-ttu-id="3574e-105">durch [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="3574e-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="3574e-106">Tailspin Spyworks wird veranschaulicht, wie außerordentlich einfach es ist, erstellen Sie leistungsstarke, skalierbare Anwendungen für die .NET-Plattform.</span><span class="sxs-lookup"><span data-stu-id="3574e-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="3574e-107">Es wird gezeigt, aus wie die hervorragenden neuen Funktionen in ASP.NET 4 zu verwenden, um eine online-Store, einschließlich der Warenkorb, Auschecken und Verwaltung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="3574e-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="3574e-108">Dieser tutorialreihe werden alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen.</span><span class="sxs-lookup"><span data-stu-id="3574e-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="3574e-109">Teil 3 beschreibt das Hinzufügen von Layout und ein kategoriemenü.</span><span class="sxs-lookup"><span data-stu-id="3574e-109">Part 3 covers adding layout and a category menu.</span></span>


## <a id="_Toc260221669"></a>  <span data-ttu-id="3574e-110">Hinzufügen von einigen Layout- und eine Kategoriemenü</span><span class="sxs-lookup"><span data-stu-id="3574e-110">Adding Some Layout and a Category Menu</span></span>

<span data-ttu-id="3574e-111">In unserem Masterseite der Website fügen wir ein DIV-Element für die Spalte der linken Seite, die unser Produkt kategoriemenü enthalten soll.</span><span class="sxs-lookup"><span data-stu-id="3574e-111">In our site master page we'll add a div for the left side column that will contain our product category menu.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample1.aspx)]

<span data-ttu-id="3574e-112">Beachten Sie, dass die gewünschte Ausrichtung und sonstigen Formatierungen von CSS-Klasse bereitgestellt werden, die wir die Datei "Style.CSS" hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="3574e-112">Note that the desired aligning and other formatting will be provided by the CSS class that we added to our Style.css file.</span></span>

[!code-css[Main](tailspin-spyworks-part-3/samples/sample2.css)]

<span data-ttu-id="3574e-113">Das Product Category-Menü wird dynamisch erstellt zur Laufzeit durch Abfragen der Commerce-Datenbank, für vorhandene Produktkategorien und erstellen die Menüelemente und entsprechenden links.</span><span class="sxs-lookup"><span data-stu-id="3574e-113">The product category menu will be dynamically created at runtime by querying the Commerce database for existing product categories and creating the menu items and corresponding links.</span></span>

<span data-ttu-id="3574e-114">Zu diesem Zweck verwenden wir zwei von ASP zu gewährleisten. NET leistungsstarke Data-Steuerelemente.</span><span class="sxs-lookup"><span data-stu-id="3574e-114">To accomplish this we will use two of ASP.NET's powerful data controls.</span></span> <span data-ttu-id="3574e-115">Das Steuerelement "Entity Data Source" und das Steuerelement "ListView".</span><span class="sxs-lookup"><span data-stu-id="3574e-115">The "Entity Data Source" control and the "ListView" control.</span></span>

![](tailspin-spyworks-part-3/_static/image1.jpg)

<span data-ttu-id="3574e-116">Wir wechseln Sie zu "Design View", und die Hilfsprogramme zur Konfiguration der unsere Steuerelemente verwenden.</span><span class="sxs-lookup"><span data-stu-id="3574e-116">Let's switch to "Design View" and use the helpers to configure our controls.</span></span>

![](tailspin-spyworks-part-3/_static/image2.jpg)

<span data-ttu-id="3574e-117">Legen wir die EntityDataSource-ID-Eigenschaft für EDS\_Kategorie\_Menü, und klicken Sie auf "Datenquelle konfigurieren".</span><span class="sxs-lookup"><span data-stu-id="3574e-117">Let's set the EntityDataSource ID property to EDS\_Category\_Menu and click on "Configure Data Source".</span></span>

![](tailspin-spyworks-part-3/_static/image3.jpg)

<span data-ttu-id="3574e-118">Wählen Sie die CommerceEntities-Verbindung, die für uns erstellt wurde, wenn wir das Entitätsdatenmodell für die Quelle für unsere Commerce-Datenbank erstellt haben, und klicken Sie auf "Weiter".</span><span class="sxs-lookup"><span data-stu-id="3574e-118">Select the CommerceEntities Connection that was created for us when we created the Entity Data Source Model for our Commerce Database and click "Next".</span></span>

![](tailspin-spyworks-part-3/_static/image4.jpg)

<span data-ttu-id="3574e-119">Wählen Sie den Namen der Entitätenmenge für "Categories", und lassen Sie die übrigen Standardeinstellungen der Optionen.</span><span class="sxs-lookup"><span data-stu-id="3574e-119">Select the "Categories" Entity set name and leave the rest of the options as default.</span></span> <span data-ttu-id="3574e-120">Klicken Sie auf "Fertig stellen".</span><span class="sxs-lookup"><span data-stu-id="3574e-120">Click "Finish".</span></span>

<span data-ttu-id="3574e-121">Jetzt legen wir die ID-Eigenschaft von der Instanz des ListView-Steuerelements, die wir auf unserer Seite aus, um die ListView platziert\_ProductsMenu und seinem Hilfsobjekt aktivieren.</span><span class="sxs-lookup"><span data-stu-id="3574e-121">Now let's set the ID property of the ListView control instance that we placed on our page to ListView\_ProductsMenu and activate its helper.</span></span>

![](tailspin-spyworks-part-3/_static/image5.jpg)

<span data-ttu-id="3574e-122">Jedoch können wir Optionen zur aufgabensteuerung zum Formatieren der Anzeige der Daten und Formatierung, unsere im Menü erstellen, benötigen nur einfache Markup damit wir den Code in der Datenquellensicht eingeben, wird.</span><span class="sxs-lookup"><span data-stu-id="3574e-122">Though we could use control options to format the data item display and formatting, our menu creation will only require simple markup so we will enter the code in the source view.</span></span>

[!code-aspx[Main](tailspin-spyworks-part-3/samples/sample3.aspx)]

<span data-ttu-id="3574e-123">Bitte beachten Sie die Anweisung "Eval": &lt;% # Eval("CategoryName") %&gt;</span><span class="sxs-lookup"><span data-stu-id="3574e-123">Please note the "Eval" statement : &lt;%# Eval("CategoryName") %&gt;</span></span>

<span data-ttu-id="3574e-124">Der ASP.NET-Syntax &lt;% # %&gt; ist eine Kurzform-Konvention, die weist die Laufzeit ausgeführt wird, was in enthalten ist, und die Ergebnisse "in Zeile".</span><span class="sxs-lookup"><span data-stu-id="3574e-124">The ASP.NET syntax &lt;%# %&gt; is a shorthand convention that instructs the runtime to execute whatever is contained within and output the results "in Line".</span></span>

<span data-ttu-id="3574e-125">Rufen Sie Eval("CategoryName") angewiesen, die für den aktuellen Eintrag in der gebundenen Auflistung von Datenelementen, den Wert der Elementnamen Entity Model "CatagoryName".</span><span class="sxs-lookup"><span data-stu-id="3574e-125">The statement Eval("CategoryName") instructs that, for the current entry in the bound collection of data items, fetch the value of the Entity Model item names "CatagoryName".</span></span> <span data-ttu-id="3574e-126">Dies ist die kürzere Syntax für ein sehr leistungsfähiges Feature.</span><span class="sxs-lookup"><span data-stu-id="3574e-126">This is concise syntax for a very powerful feature.</span></span>

<span data-ttu-id="3574e-127">Können die Anwendung nun ausführen.</span><span class="sxs-lookup"><span data-stu-id="3574e-127">Lets run the application now.</span></span>

![](tailspin-spyworks-part-3/_static/image6.jpg)

<span data-ttu-id="3574e-128">Beachten Sie, dass unser Produkt kategoriemenü wird nun angezeigt, wenn wir über eine Kategorie Menüelemente zeigen sehen wir, die im Menü Element Link verweist auf eine Seite, die wir noch implementieren müssen, mit dem Namen ProductsList.aspx und, wir ein Argument für eine dynamische Abfrage erstellt haben, enthält die  Kategorie-Id.</span><span class="sxs-lookup"><span data-stu-id="3574e-128">Note that our product category menu is now displayed and when we hover over one of the category menu items we can see the menu item link points to a page we have yet to implement named ProductsList.aspx and that we have built a dynamic query string argument that contains the category id.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3574e-129">[Zurück](tailspin-spyworks-part-2.md)
> [Weiter](tailspin-spyworks-part-4.md)</span><span class="sxs-lookup"><span data-stu-id="3574e-129">[Previous](tailspin-spyworks-part-2.md)
[Next](tailspin-spyworks-part-4.md)</span></span>
