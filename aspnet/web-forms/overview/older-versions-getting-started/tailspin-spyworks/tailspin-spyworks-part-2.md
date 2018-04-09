---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
title: 'Teil 2: Datenzugriffsebene | Microsoft Docs'
author: JoeStagner
description: Diese Reihe von Lernprogrammen sind alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen. Teil 2 wird das Hinzufügen von der Datenzugriffsebene behandelt.
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 5a9d5429-d70b-411c-8474-f42cf7ef8a2b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-2
msc.type: authoredcontent
ms.openlocfilehash: 9f734b04a0f4cec3c33bc5b42ef283ea64cdb463
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="part-2-data-access-layer"></a><span data-ttu-id="97524-104">Teil 2: Die Datenzugriffsebene</span><span class="sxs-lookup"><span data-stu-id="97524-104">Part 2: Data Access Layer</span></span>
====================
<span data-ttu-id="97524-105">durch [Joe Stagner](https://github.com/JoeStagner)</span><span class="sxs-lookup"><span data-stu-id="97524-105">by [Joe Stagner](https://github.com/JoeStagner)</span></span>

> <span data-ttu-id="97524-106">Tailspin Spyworks wird veranschaulicht, wie außergewöhnlich einfache ist die leistungsstarke, skalierbare Anwendungen für .NET-Plattform zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="97524-106">Tailspin Spyworks demonstrates how extraordinarily simple it is to create powerful, scalable applications for the .NET platform.</span></span> <span data-ttu-id="97524-107">Es wird gezeigt, aus wie die hervorragenden neuen Funktionen in ASP.NET 4 mit um einen Onlineshop, einschließlich Warenkorb, Auschecken und Verwaltung zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="97524-107">It shows off how to use the great new features in ASP.NET 4 to build an online store, including shopping, checkout, and administration.</span></span>
> 
> <span data-ttu-id="97524-108">Diese Reihe von Lernprogrammen sind alle Schritte ausgeführt, um die beispielanwendung Tailspin Spyworks erstellen.</span><span class="sxs-lookup"><span data-stu-id="97524-108">This tutorial series details all of the steps taken to build the Tailspin Spyworks sample application.</span></span> <span data-ttu-id="97524-109">Teil 2 wird das Hinzufügen von der Datenzugriffsebene behandelt.</span><span class="sxs-lookup"><span data-stu-id="97524-109">Part 2 covers adding the data access layer.</span></span>


## <a id="_Toc260221668"></a>  <span data-ttu-id="97524-110">Hinzufügen von der Datenzugriffsebene</span><span class="sxs-lookup"><span data-stu-id="97524-110">Adding the Data Access Layer</span></span>

<span data-ttu-id="97524-111">Unsere e-Commerce-Anwendung hängt von zwei Datenbanken.</span><span class="sxs-lookup"><span data-stu-id="97524-111">Our ecommerce application will depend on two databases.</span></span>

<span data-ttu-id="97524-112">Für Kundeninformationen verwenden wir die ASP.NET-Mitgliedschaft-Standarddatenbank.</span><span class="sxs-lookup"><span data-stu-id="97524-112">For customer information we'll use the standard ASP.NET Membership database.</span></span> <span data-ttu-id="97524-113">Für unsere Warenkorb Einkaufswagen und Product Catalog implementieren wir eine SQL Express-Datenbank wie folgt.</span><span class="sxs-lookup"><span data-stu-id="97524-113">For our shopping cart and product catalog we'll implement a SQL Express database as follows.</span></span>

![](tailspin-spyworks-part-2/_static/image1.jpg)

<span data-ttu-id="97524-114">Mit dem Erstellen der Datenbank (Commerce.mdf) in der Anwendungskonfigurationsdatei App\_Datenordner können wir unsere Datenzugriffsebene, die mit dem .NET Entity Framework erstellen fortfahren.</span><span class="sxs-lookup"><span data-stu-id="97524-114">Having created the database (Commerce.mdf) in the application's App\_Data folder we can proceed to create our Data Access Layer using the .NET Entity Framework.</span></span>

<span data-ttu-id="97524-115">Wir erstellen einen Ordner mit dem Namen "Data\_Zugriff" und sie mit der rechten Maustaste auf den Ordner, und wählen Sie "Neues Element hinzufügen".</span><span class="sxs-lookup"><span data-stu-id="97524-115">We'll create a folder named "Data\_Access" and them right click on that folder and select "Add New Item".</span></span>

<span data-ttu-id="97524-116">Geben Sie unter "Installierte Vorlagen" Element und wählen Sie dann "ADO.NET Entity Data Model" EDM\_Commerce.edmx als den Namen, und klicken Sie auf die Schaltfläche "Hinzufügen".</span><span class="sxs-lookup"><span data-stu-id="97524-116">In the "Installed Templates" item and then select "ADO.NET Entity Data Model" enter EDM\_Commerce.edmx as the name and click the "Add" button.</span></span>

![](tailspin-spyworks-part-2/_static/image2.jpg)

<span data-ttu-id="97524-117">Wählen Sie "Aus Datenbank generieren".</span><span class="sxs-lookup"><span data-stu-id="97524-117">Choose "Generate from Database".</span></span>

![](tailspin-spyworks-part-2/_static/image1.png)

![](tailspin-spyworks-part-2/_static/image2.png)

![](tailspin-spyworks-part-2/_static/image3.png)

![](tailspin-spyworks-part-2/_static/image3.jpg)

<span data-ttu-id="97524-118">Speichern und erstellen.</span><span class="sxs-lookup"><span data-stu-id="97524-118">Save and build.</span></span>

<span data-ttu-id="97524-119">Jetzt können wir unsere erste Funktion – ein Product Category-Menü hinzu.</span><span class="sxs-lookup"><span data-stu-id="97524-119">Now we are ready to add our first feature – a product category menu.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="97524-120">[Zurück](tailspin-spyworks-part-1.md)
> [Weiter](tailspin-spyworks-part-3.md)</span><span class="sxs-lookup"><span data-stu-id="97524-120">[Previous](tailspin-spyworks-part-1.md)
[Next](tailspin-spyworks-part-3.md)</span></span>
