---
uid: mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
title: Erstellen eines Domänencontrollers (VB) | Microsoft Docs
author: StephenWalther
description: In diesem Lernprogramm wird Stephen Walther veranschaulicht, wie Sie einen Controller einer ASP.NET MVC-Anwendung hinzufügen können.
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 204b7e86-f560-4611-8adb-785b33e777b9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/creating-a-controller-vb
msc.type: authoredcontent
ms.openlocfilehash: e9a2bbcb09672f5247429064908cd4d2ef67f518
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879009"
---
<a name="creating-a-controller-vb"></a><span data-ttu-id="4aca5-103">Erstellen eines Domänencontrollers (VB)</span><span class="sxs-lookup"><span data-stu-id="4aca5-103">Creating a Controller (VB)</span></span>
====================
<span data-ttu-id="4aca5-104">durch [Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="4aca5-104">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="4aca5-105">In diesem Lernprogramm wird Stephen Walther veranschaulicht, wie Sie einen Controller einer ASP.NET MVC-Anwendung hinzufügen können.</span><span class="sxs-lookup"><span data-stu-id="4aca5-105">In this tutorial, Stephen Walther demonstrates how you can add a controller to an ASP.NET MVC application.</span></span>


<span data-ttu-id="4aca5-106">Ziel dieses Lernprogramms wird erläutert, wie Sie die neue ASP.NET MVC Controllers erstellen können.</span><span class="sxs-lookup"><span data-stu-id="4aca5-106">The goal of this tutorial is to explain how you can create new ASP.NET MVC controllers.</span></span> <span data-ttu-id="4aca5-107">Erfahren Sie, wie zum Erstellen von Controllern mithilfe der Visual Studio, Controller hinzufügen "und erstellen eine Klassendatei per hand katalogisiert wird.</span><span class="sxs-lookup"><span data-stu-id="4aca5-107">You learn how to create controllers both by using the Visual Studio Add Controller menu option and by creating a class file by hand.</span></span>

### <a name="using-the-add-controller-menu-option"></a><span data-ttu-id="4aca5-108">Mit der Menüoption Controller hinzufügen</span><span class="sxs-lookup"><span data-stu-id="4aca5-108">Using the Add Controller Menu Option</span></span>

<span data-ttu-id="4aca5-109">Die einfachste Möglichkeit zum Erstellen eines neuen Controllers ist mit der rechten Maustaste in des Ordners "Controller" im Projektmappen-Explorer von Visual Studio-Fenster, und wählen Sie die **hinzufügen, Controller** Menüoption (siehe Abbildung 1).</span><span class="sxs-lookup"><span data-stu-id="4aca5-109">The easiest way to create a new controller is to right-click the Controllers folder in the Visual Studio Solution Explorer window and select the **Add, Controller** menu option (see Figure 1).</span></span> <span data-ttu-id="4aca5-110">Durch das Auswählen dieser Option wird die **Controller hinzufügen** Dialogfeld (siehe Abbildung 2).</span><span class="sxs-lookup"><span data-stu-id="4aca5-110">Selecting this menu option opens the **Add Controller** dialog (see Figure 2).</span></span>


<span data-ttu-id="4aca5-111">[![Das Dialogfeld "Neues Projekt"](creating-a-controller-vb/_static/image1.jpg)](creating-a-controller-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4aca5-111">[![The New Project dialog box](creating-a-controller-vb/_static/image1.jpg)](creating-a-controller-vb/_static/image1.png)</span></span>

<span data-ttu-id="4aca5-112">**Abbildung 01**: Hinzufügen eines neuen Controllers ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-controller-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="4aca5-112">**Figure 01**: Adding a new controller([Click to view full-size image](creating-a-controller-vb/_static/image2.png))</span></span>


<span data-ttu-id="4aca5-113">[![Das Dialogfeld "Neues Projekt"](creating-a-controller-vb/_static/image2.jpg)](creating-a-controller-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="4aca5-113">[![The New Project dialog box](creating-a-controller-vb/_static/image2.jpg)](creating-a-controller-vb/_static/image3.png)</span></span>

<span data-ttu-id="4aca5-114">**Abbildung 02**: der Controller der hinzufügen-Dialogfeld ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-controller-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="4aca5-114">**Figure 02**: The Add Controller dialog ([Click to view full-size image](creating-a-controller-vb/_static/image4.png))</span></span>


<span data-ttu-id="4aca5-115">Beachten Sie, die in der erste Teil des Controllernamens hervorgehoben ist die **Controller hinzufügen** Dialogfeld.</span><span class="sxs-lookup"><span data-stu-id="4aca5-115">Notice that the first part of the controller name is highlighted in the **Add Controller** dialog.</span></span> <span data-ttu-id="4aca5-116">Jeder Controllername muss mit dem Suffix enden *Controller*.</span><span class="sxs-lookup"><span data-stu-id="4aca5-116">Every controller name must end with the suffix *Controller*.</span></span> <span data-ttu-id="4aca5-117">Sie können z. B. einen Controller namens erstellen *ProductController* jedoch nicht auf einen Controller aus, die mit dem Namen *Produkt*.</span><span class="sxs-lookup"><span data-stu-id="4aca5-117">For example, you can create a controller named *ProductController* but not a controller named *Product*.</span></span>


<span data-ttu-id="4aca5-118">Wenn Sie einen Controller erstellen, die fehlen die *Controller* suffix, und Sie können zum Aufrufen des Controllers nicht.</span><span class="sxs-lookup"><span data-stu-id="4aca5-118">If you create a controller that is missing the *Controller* suffix then you won't be able to invoke the controller.</span></span> <span data-ttu-id="4aca5-119">Führen Sie dies – ich haben zahllose Stunden damit Meine abgelaufenen nach dem Herstellen dieser Fehler verschwendet.</span><span class="sxs-lookup"><span data-stu-id="4aca5-119">Don't do this -- I've wasted countless hours of my life after making this mistake.</span></span>


<span data-ttu-id="4aca5-120">**1 – Controllers\ProductController.vb auflisten**</span><span class="sxs-lookup"><span data-stu-id="4aca5-120">**Listing 1 - Controllers\ProductController.vb**</span></span>

[!code-vb[Main](creating-a-controller-vb/samples/sample1.vb)]

<span data-ttu-id="4aca5-121">Sie sollten Domänencontroller immer im Ordner "Controller" erstellen.</span><span class="sxs-lookup"><span data-stu-id="4aca5-121">You should always create controllers in the Controllers folder.</span></span> <span data-ttu-id="4aca5-122">Andernfalls Sie müssen die Konventionen der ASP.NET MVC verletzen, und andere Entwickler müssen nacheinander schwieriger verstehen Ihre Anwendung.</span><span class="sxs-lookup"><span data-stu-id="4aca5-122">Otherwise, you'll be violating the conventions of ASP.NET MVC and other developers will have a more difficult time understanding your application.</span></span>

### <a name="scaffolding-action-methods"></a><span data-ttu-id="4aca5-123">Gerüstbau Aktionsmethoden</span><span class="sxs-lookup"><span data-stu-id="4aca5-123">Scaffolding Action Methods</span></span>

<span data-ttu-id="4aca5-124">Wenn Sie einen Controller erstellen, stehen Ihnen die Option zum automatischen Generieren von erstellen, aktualisieren und Details Aktionsmethoden (siehe Abbildung 3).</span><span class="sxs-lookup"><span data-stu-id="4aca5-124">When you create a controller, you have the option to generate Create, Update, and Details action methods automatically (see Figure 3).</span></span> <span data-ttu-id="4aca5-125">Bei Auswahl dieser Option wird die Controllerklasse auflisten 2 generiert.</span><span class="sxs-lookup"><span data-stu-id="4aca5-125">If you select this option then the controller class in Listing 2 is generated.</span></span>


<span data-ttu-id="4aca5-126">[![Aktionsmethoden erstellen automatisch](creating-a-controller-vb/_static/image3.jpg)](creating-a-controller-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="4aca5-126">[![Creating action methods automatically](creating-a-controller-vb/_static/image3.jpg)](creating-a-controller-vb/_static/image5.png)</span></span>

<span data-ttu-id="4aca5-127">**Abbildung 03**: Aktionsmethoden automatisch erstellt ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-controller-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="4aca5-127">**Figure 03**: Creating action methods automatically ([Click to view full-size image](creating-a-controller-vb/_static/image6.png))</span></span>


<span data-ttu-id="4aca5-128">**Auflisten von 2 – Controllers\CustomerController.vb**</span><span class="sxs-lookup"><span data-stu-id="4aca5-128">**Listing 2 - Controllers\CustomerController.vb**</span></span>

[!code-vb[Main](creating-a-controller-vb/samples/sample2.vb)]

<span data-ttu-id="4aca5-129">Diese generierten Methoden werden Stub.</span><span class="sxs-lookup"><span data-stu-id="4aca5-129">These generated methods are stub methods.</span></span> <span data-ttu-id="4aca5-130">Sie müssen die eigentliche Logik zum Erstellen, aktualisieren und Anzeigen von Details für einen Kunden selbst hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="4aca5-130">You must add the actual logic for creating, updating, and showing details for a customer yourself.</span></span> <span data-ttu-id="4aca5-131">Allerdings die Stubmethoden mit nice Ausgangspunkt bereitgestellt.</span><span class="sxs-lookup"><span data-stu-id="4aca5-131">But, the stub methods provide you with a nice starting point.</span></span>

### <a name="creating-a-controller-class"></a><span data-ttu-id="4aca5-132">Erstellen einer Controllerklasse</span><span class="sxs-lookup"><span data-stu-id="4aca5-132">Creating a Controller Class</span></span>

<span data-ttu-id="4aca5-133">Der ASP.NET MVC-Controller ist nur eine Klasse.</span><span class="sxs-lookup"><span data-stu-id="4aca5-133">The ASP.NET MVC controller is just a class.</span></span> <span data-ttu-id="4aca5-134">Falls gewünscht, können Sie ignorieren die geeigneten Controller Gerüstbau für Visual Studio und erstellen eine Controllerklasse per hand katalogisiert wird.</span><span class="sxs-lookup"><span data-stu-id="4aca5-134">If you prefer, you can ignore the convenient Visual Studio controller scaffolding and create a controller class by hand.</span></span> <span data-ttu-id="4aca5-135">Führen Sie folgende Schritte aus:</span><span class="sxs-lookup"><span data-stu-id="4aca5-135">Follow these steps:</span></span>

1. <span data-ttu-id="4aca5-136">Mit der rechten Maustaste in des Ordners Controller, und wählen Sie die Menüoption **hinzufügen, neue Element** , und wählen Sie die **Klasse** Vorlage (siehe Abbildung 4).</span><span class="sxs-lookup"><span data-stu-id="4aca5-136">Right-click the Controllers folder and select the menu option **Add, New Item** and select the **Class** template (see Figure 4).</span></span>
2. <span data-ttu-id="4aca5-137">Benennen Sie die neue Klasse PersonController.vb, und klicken Sie auf die **hinzufügen** Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="4aca5-137">Name the new class PersonController.vb and click the **Add** button.</span></span>
3. <span data-ttu-id="4aca5-138">Ändern Sie die resultierende Klassendatei, sodass die Klasse von der Basisklasse System.Web.Mvc.Controller (Siehe auflisten von 3) erbt.</span><span class="sxs-lookup"><span data-stu-id="4aca5-138">Modify the resulting class file so that the class inherits from the base System.Web.Mvc.Controller class (see Listing 3).</span></span>


<span data-ttu-id="4aca5-139">[![Erstellen einer neuen Klasse](creating-a-controller-vb/_static/image4.jpg)](creating-a-controller-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="4aca5-139">[![Creating a new class](creating-a-controller-vb/_static/image4.jpg)](creating-a-controller-vb/_static/image7.png)</span></span>

<span data-ttu-id="4aca5-140">**Abbildung 04**: Erstellen einer neuen Klasse ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-a-controller-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="4aca5-140">**Figure 04**: Creating a new class([Click to view full-size image](creating-a-controller-vb/_static/image8.png))</span></span>


<span data-ttu-id="4aca5-141">**3 – Controllers\PersonController.vb auflisten**</span><span class="sxs-lookup"><span data-stu-id="4aca5-141">**Listing 3 - Controllers\PersonController.vb**</span></span>

[!code-vb[Main](creating-a-controller-vb/samples/sample3.vb)]

<span data-ttu-id="4aca5-142">Der Controller im Codebeispiel 3 verfügbar macht, eine Aktion, die mit dem Namen Index(), die die Zeichenfolge "Hello World!" zurückgibt.</span><span class="sxs-lookup"><span data-stu-id="4aca5-142">The controller in Listing 3 exposes one action named Index() that returns the string "Hello World!".</span></span> <span data-ttu-id="4aca5-143">Sie können diese Controlleraktion durch Ausführen der Anwendung und zum Anfordern einer URL wie folgt aufrufen:</span><span class="sxs-lookup"><span data-stu-id="4aca5-143">You can invoke this controller action by running your application and requesting a URL like the following:</span></span>

`http://localhost:40071/Person`

> [!NOTE]
> 
> <span data-ttu-id="4aca5-144">Der ASP.NET Development Server verwendet eine zufällige Portnummer (z. B. 40071).</span><span class="sxs-lookup"><span data-stu-id="4aca5-144">The ASP.NET Development Server uses a random port number (for example, 40071).</span></span> <span data-ttu-id="4aca5-145">Wenn Sie eine URL zum Aufrufen eines Controllers eingeben, müssen Sie die richtige Portnummer angeben.</span><span class="sxs-lookup"><span data-stu-id="4aca5-145">When entering a URL to invoke a controller, you'll need to supply the right port number.</span></span> <span data-ttu-id="4aca5-146">Sie können die Nummer des Ports bestimmen, indem Sie mit dem Mauszeiger der Maus auf das Symbol für den ASP.NET Development Server im Infobereich von Windows (untere Ecke des Bildschirms).</span><span class="sxs-lookup"><span data-stu-id="4aca5-146">You can determine the port number by hovering your mouse over the icon for the ASP.NET Development Server in the Windows Notification Area (bottom-right of your screen).</span></span>
> 
> [!div class="step-by-step"]
> <span data-ttu-id="4aca5-147">[Zurück](adding-dynamic-content-to-a-cached-page-vb.md)
> [Weiter](creating-an-action-vb.md)</span><span class="sxs-lookup"><span data-stu-id="4aca5-147">[Previous](adding-dynamic-content-to-a-cached-page-vb.md)
[Next](creating-an-action-vb.md)</span></span>
