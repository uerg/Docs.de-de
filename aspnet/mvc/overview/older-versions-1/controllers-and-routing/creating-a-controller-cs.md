---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-cs
title: Erstellen eines Controllers (c#) | Microsoft-Dokumentation
author: StephenWalther
description: Stephen Walther wird in diesem Tutorial veranschaulicht, wie Sie einen Controller zu einer ASP.NET MVC-Anwendung hinzufügen können.
ms.author: aspnetcontent
ms.date: 03/02/2009
ms.assetid: 719d50d4-2305-454c-98b4-bae64937c48f
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-cs
msc.type: authoredcontent
ms.openlocfilehash: 3b3b663f76b6271a213e5980756ba63372ef6fe9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804264"
---
<a name="creating-a-controller-c"></a><span data-ttu-id="f1871-103">Erstellen eines Controllers (c#)</span><span class="sxs-lookup"><span data-stu-id="f1871-103">Creating a Controller (C#)</span></span>
====================
<span data-ttu-id="f1871-104">durch [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="f1871-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="f1871-105">Stephen Walther wird in diesem Tutorial veranschaulicht, wie Sie einen Controller zu einer ASP.NET MVC-Anwendung hinzufügen können.</span><span class="sxs-lookup"><span data-stu-id="f1871-105">In this tutorial, Stephen Walther demonstrates how you can add a controller to an ASP.NET MVC application.</span></span>


<span data-ttu-id="f1871-106">Das Ziel in diesem Tutorial wird beschrieben, wie Sie die neue ASP.NET MVC Controllers erstellen können.</span><span class="sxs-lookup"><span data-stu-id="f1871-106">The goal of this tutorial is to explain how you can create new ASP.NET MVC controllers.</span></span> <span data-ttu-id="f1871-107">Erfahren Sie, wie Controller nach mithilfe der Visual Studio, Controller hinzufügen "-" und erstellen eine Datei manuell zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="f1871-107">You learn how to create controllers both by using the Visual Studio Add Controller menu option and by creating a class file by hand.</span></span>

### <a name="using-the-add-controller-menu-option"></a><span data-ttu-id="f1871-108">Mithilfe der Menüoption Controller hinzufügen</span><span class="sxs-lookup"><span data-stu-id="f1871-108">Using the Add Controller Menu Option</span></span>

<span data-ttu-id="f1871-109">Die einfachste Möglichkeit zum Erstellen eines neuen Controllers ist mit der rechten Maustaste in den Ordner "Controllers" im Projektmappen-Explorer von Visual Studio-Fenster, und wählen Sie die **hinzufügen, Controller** Menüoption (siehe Abbildung 1).</span><span class="sxs-lookup"><span data-stu-id="f1871-109">The easiest way to create a new controller is to right-click the Controllers folder in the Visual Studio Solution Explorer window and select the **Add, Controller** menu option (see Figure 1).</span></span> <span data-ttu-id="f1871-110">Durch Auswählen dieser Menüoption wird das **Controller hinzufügen** Dialogfeld (siehe Abbildung 2).</span><span class="sxs-lookup"><span data-stu-id="f1871-110">Selecting this menu option opens the **Add Controller** dialog (see Figure 2).</span></span>


<span data-ttu-id="f1871-111">[![Das Dialogfeld "Neues Projekt"](creating-a-controller-cs/_static/image1.jpg)](creating-a-controller-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f1871-111">[![The New Project dialog box](creating-a-controller-cs/_static/image1.jpg)](creating-a-controller-cs/_static/image1.png)</span></span>

<span data-ttu-id="f1871-112">**Abbildung 01**: Hinzufügen eines neuen Controllers ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-controller-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="f1871-112">**Figure 01**: Adding a new controller([Click to view full-size image](creating-a-controller-cs/_static/image2.png))</span></span>


<span data-ttu-id="f1871-113">[![Das Dialogfeld "Neues Projekt"](creating-a-controller-cs/_static/image2.jpg)](creating-a-controller-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="f1871-113">[![The New Project dialog box](creating-a-controller-cs/_static/image2.jpg)](creating-a-controller-cs/_static/image3.png)</span></span>

<span data-ttu-id="f1871-114">**Abbildung 02**: die Controller der hinzufügen-Dialogfeld ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-controller-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="f1871-114">**Figure 02**: The Add Controller dialog ([Click to view full-size image](creating-a-controller-cs/_static/image4.png))</span></span>


<span data-ttu-id="f1871-115">Beachten Sie, das der erste Teil des Controllernamens, in ausgewählt ist der **Controller hinzufügen** Dialogfeld.</span><span class="sxs-lookup"><span data-stu-id="f1871-115">Notice that the first part of the controller name is highlighted in the **Add Controller** dialog.</span></span> <span data-ttu-id="f1871-116">Jeder Controllername muss mit der Endung *Controller*.</span><span class="sxs-lookup"><span data-stu-id="f1871-116">Every controller name must end with the suffix *Controller*.</span></span> <span data-ttu-id="f1871-117">Sie können z. B. einen Controller namens erstellen *ProductController* jedoch nicht auf einen Controller, mit dem Namen *Produkt*.</span><span class="sxs-lookup"><span data-stu-id="f1871-117">For example, you can create a controller named *ProductController* but not a controller named *Product*.</span></span>


<span data-ttu-id="f1871-118">Wenn Sie einen Controller erstellen, die fehlen die *Controller* suffix, und klicken Sie dann Sie nicht den Controller aufrufen können.</span><span class="sxs-lookup"><span data-stu-id="f1871-118">If you create a controller that is missing the *Controller* suffix then you won't be able to invoke the controller.</span></span> <span data-ttu-id="f1871-119">Davon wird abgeraten, denn ich haben sich zahllose Stunden für mein Leben, nachdem Sie diesen Fehler, verschwendet.</span><span class="sxs-lookup"><span data-stu-id="f1871-119">Don't do this -- I've wasted countless hours of my life after making this mistake.</span></span>


<span data-ttu-id="f1871-120">**1 – Controllers\ProductController.cs auflisten**</span><span class="sxs-lookup"><span data-stu-id="f1871-120">**Listing 1 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](creating-a-controller-cs/samples/sample1.cs)]

<span data-ttu-id="f1871-121">Sie sollten immer im Ordner "Controllers" Controller erstellen.</span><span class="sxs-lookup"><span data-stu-id="f1871-121">You should always create controllers in the Controllers folder.</span></span> <span data-ttu-id="f1871-122">Andernfalls Sie verletzt werden die Konventionen der ASP.NET MVC und andere Entwickler haben schwieriger verstehen Ihre Anwendung.</span><span class="sxs-lookup"><span data-stu-id="f1871-122">Otherwise, you'll be violating the conventions of ASP.NET MVC and other developers will have a more difficult time understanding your application.</span></span>

### <a name="scaffolding-action-methods"></a><span data-ttu-id="f1871-123">Gerüstbau für Aktionsmethoden</span><span class="sxs-lookup"><span data-stu-id="f1871-123">Scaffolding Action Methods</span></span>

<span data-ttu-id="f1871-124">Wenn Sie einen Controller erstellen, haben Sie die Option zum automatischen Generieren von Aktionsmethoden für Create, Update und Details (siehe Abbildung 3).</span><span class="sxs-lookup"><span data-stu-id="f1871-124">When you create a controller, you have the option to generate Create, Update, and Details action methods automatically (see Figure 3).</span></span> <span data-ttu-id="f1871-125">Bei Auswahl dieser Option wird die Controller-Klasse im Codebeispiel 2 generiert.</span><span class="sxs-lookup"><span data-stu-id="f1871-125">If you select this option then the controller class in Listing 2 is generated.</span></span>


<span data-ttu-id="f1871-126">[![Aktionsmethoden erstellen automatisch](creating-a-controller-cs/_static/image3.jpg)](creating-a-controller-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="f1871-126">[![Creating action methods automatically](creating-a-controller-cs/_static/image3.jpg)](creating-a-controller-cs/_static/image5.png)</span></span>

<span data-ttu-id="f1871-127">**Abbildung 03**: Aktionsmethoden automatisch erstellt ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-controller-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="f1871-127">**Figure 03**: Creating action methods automatically ([Click to view full-size image](creating-a-controller-cs/_static/image6.png))</span></span>


<span data-ttu-id="f1871-128">**Codebeispiel 2 - Controllers\CustomerController.cs**</span><span class="sxs-lookup"><span data-stu-id="f1871-128">**Listing 2 - Controllers\CustomerController.cs**</span></span>

[!code-csharp[Main](creating-a-controller-cs/samples/sample2.cs)]

<span data-ttu-id="f1871-129">Diese generierten Methoden sind Stubmethoden.</span><span class="sxs-lookup"><span data-stu-id="f1871-129">These generated methods are stub methods.</span></span> <span data-ttu-id="f1871-130">Sie müssen die eigentliche Logik zum Erstellen, aktualisieren und Anzeigen von Details für einen Kunden selbst hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="f1871-130">You must add the actual logic for creating, updating, and showing details for a customer yourself.</span></span> <span data-ttu-id="f1871-131">Aber die Stubmethoden bieten Ihnen gute Ausgangspunkt.</span><span class="sxs-lookup"><span data-stu-id="f1871-131">But, the stub methods provide you with a nice starting point.</span></span>

### <a name="creating-a-controller-class"></a><span data-ttu-id="f1871-132">Erstellen eine Controller-Klasse</span><span class="sxs-lookup"><span data-stu-id="f1871-132">Creating a Controller Class</span></span>

<span data-ttu-id="f1871-133">Die ASP.NET MVC-Controller ist nur eine Klasse.</span><span class="sxs-lookup"><span data-stu-id="f1871-133">The ASP.NET MVC controller is just a class.</span></span> <span data-ttu-id="f1871-134">Falls gewünscht, können Sie ignorieren den einfachen Controller Gerüstbau für Visual Studio und erstellen manuell eine Controller-Klasse.</span><span class="sxs-lookup"><span data-stu-id="f1871-134">If you prefer, you can ignore the convenient Visual Studio controller scaffolding and create a controller class by hand.</span></span> <span data-ttu-id="f1871-135">Führen Sie folgende Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="f1871-135">Follow these steps:</span></span>

1. <span data-ttu-id="f1871-136">Mit der rechten Maustaste in den Ordner "Controllers", und wählen Sie die Menüoption **hinzufügen, neue Element** , und wählen Sie die **Klasse** Vorlage (siehe Abbildung 4).</span><span class="sxs-lookup"><span data-stu-id="f1871-136">Right-click the Controllers folder and select the menu option **Add, New Item** and select the **Class** template (see Figure 4).</span></span>
2. <span data-ttu-id="f1871-137">Nennen Sie die neue Klasse PersonController.cs, und klicken Sie auf die **hinzufügen** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="f1871-137">Name the new class PersonController.cs and click the **Add** button.</span></span>
3. <span data-ttu-id="f1871-138">Ändern Sie die resultierende Klassendatei, sodass die Klasse von der Basisklasse für den System.Web.Mvc.Controller (siehe Codebeispiel 3) erbt.</span><span class="sxs-lookup"><span data-stu-id="f1871-138">Modify the resulting class file so that the class inherits from the base System.Web.Mvc.Controller class (see Listing 3).</span></span>


<span data-ttu-id="f1871-139">[![Erstellen einer neuen Klasse](creating-a-controller-cs/_static/image4.jpg)](creating-a-controller-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="f1871-139">[![Creating a new class](creating-a-controller-cs/_static/image4.jpg)](creating-a-controller-cs/_static/image7.png)</span></span>

<span data-ttu-id="f1871-140">**Abbildung 04**: Erstellen einer neuen Klasse ([klicken Sie, um das Bild in voller Größe anzeigen](creating-a-controller-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="f1871-140">**Figure 04**: Creating a new class([Click to view full-size image](creating-a-controller-cs/_static/image8.png))</span></span>


<span data-ttu-id="f1871-141">**Codebeispiel 3 - Controllers\PersonController.cs**</span><span class="sxs-lookup"><span data-stu-id="f1871-141">**Listing 3 - Controllers\PersonController.cs**</span></span>

[!code-csharp[Main](creating-a-controller-cs/samples/sample3.cs)]

<span data-ttu-id="f1871-142">Der Controller in Programmausdruck 3 bietet es sich um eine Aktion, die mit dem Namen Index(), die die Zeichenfolge "Hello World!" zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="f1871-142">The controller in Listing 3 exposes one action named Index() that returns the string "Hello World!".</span></span> <span data-ttu-id="f1871-143">Sie können diese Controlleraktion aufrufen, indem Sie Ausführung Ihrer Anwendung und zum Anfordern einer URL wie folgt:</span><span class="sxs-lookup"><span data-stu-id="f1871-143">You can invoke this controller action by running your application and requesting a URL like the following:</span></span>

`http://localhost:40071/Person`

> [!NOTE]
> 
> <span data-ttu-id="f1871-144">Der ASP.NET Development Server verwendet eine beliebige Portnummer (z. B. 40071).</span><span class="sxs-lookup"><span data-stu-id="f1871-144">The ASP.NET Development Server uses a random port number (for example, 40071).</span></span> <span data-ttu-id="f1871-145">Wenn Sie eine URL zum Aufrufen eines Controllers eingeben zu können, müssen Sie die richtige Portnummer angeben.</span><span class="sxs-lookup"><span data-stu-id="f1871-145">When entering a URL to invoke a controller, you'll need to supply the right port number.</span></span> <span data-ttu-id="f1871-146">Sie können die Nummer des Ports bestimmen, durch Bewegen des Mauszeigers über dem Symbol für den ASP.NET Development Server in den Windows-Benachrichtigungsbereich (unten rechts des Bildschirms).</span><span class="sxs-lookup"><span data-stu-id="f1871-146">You can determine the port number by hovering your mouse over the icon for the ASP.NET Development Server in the Windows Notification Area (bottom-right of your screen).</span></span>
> 
> [!div class="step-by-step"]
> <span data-ttu-id="f1871-147">[Zurück](adding-dynamic-content-to-a-cached-page-cs.md)
> [Weiter](creating-an-action-cs.md)</span><span class="sxs-lookup"><span data-stu-id="f1871-147">[Previous](adding-dynamic-content-to-a-cached-page-cs.md)
[Next](creating-an-action-cs.md)</span></span>
