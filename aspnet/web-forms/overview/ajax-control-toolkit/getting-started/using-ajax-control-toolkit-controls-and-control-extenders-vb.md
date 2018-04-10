---
uid: web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-vb
title: Verwenden von AJAX-Steuerelement-Toolkit-Steuerelemente und -Extender (VB) | Microsoft Docs
author: microsoft
description: Erfahren Sie, wie ASP.NET-Seiten AJAX-Steuerelement-Toolkit-Steuerelemente und Extender hinzugefügt.
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/12/2009
ms.topic: article
ms.assetid: 763650a9-ffde-46a9-b779-7a9145dd5d88
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/getting-started/using-ajax-control-toolkit-controls-and-control-extenders-vb
msc.type: authoredcontent
ms.openlocfilehash: 080dd65677d80fb75ab37a20f6c385a38af4e353
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="using-ajax-control-toolkit-controls-and-control-extenders-vb"></a><span data-ttu-id="25619-103">Verwenden von AJAX-Steuerelement-Toolkit-Steuerelemente und -Extender (VB)</span><span class="sxs-lookup"><span data-stu-id="25619-103">Using AJAX Control Toolkit Controls and Control Extenders (VB)</span></span>
====================
<span data-ttu-id="25619-104">durch [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="25619-104">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="25619-105">Erfahren Sie, wie ASP.NET-Seiten AJAX-Steuerelement-Toolkit-Steuerelemente und Extender hinzugefügt.</span><span class="sxs-lookup"><span data-stu-id="25619-105">Learn how to add AJAX Control Toolkit controls and extenders to your ASP.NET pages.</span></span>


<span data-ttu-id="25619-106">Das AJAX-Steuerelement-Toolkit enthält einen Satz von Steuerelementen und -Extender.</span><span class="sxs-lookup"><span data-stu-id="25619-106">The AJAX Control Toolkit contains a set of controls and control extenders.</span></span> <span data-ttu-id="25619-107">In diesem kurzen Tutorial lernen Sie zum Hinzufügen von Steuerelementen und -Extender zu einer ASP.NET-Seite.</span><span class="sxs-lookup"><span data-stu-id="25619-107">In this brief tutorial, you learn how to add both controls and control extenders to an ASP.NET page.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="25619-108">Für Anweisungen zum Installieren des AJAX-Steuerelement-Toolkit und Hinzufügen von AJAX-Steuerelement-Toolkit zur Visual Studio/Visual Web Developer-Toolbox finden Sie im Lernprogramm [erste Schritte mit AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-vb.md).</span><span class="sxs-lookup"><span data-stu-id="25619-108">For instructions on installing the AJAX Control Toolkit and adding the AJAX Control Toolkit to the Visual Studio/Visual Web Developer toolbox, see the tutorial [Get Started with the AJAX Control Toolkit](get-started-with-the-ajax-control-toolkit-vb.md).</span></span>


## <a name="using-ajax-control-toolkit-controls"></a><span data-ttu-id="25619-109">Verwenden von AJAX-Steuerelement-Toolkit-Steuerelemente</span><span class="sxs-lookup"><span data-stu-id="25619-109">Using AJAX Control Toolkit Controls</span></span>

<span data-ttu-id="25619-110">Ein AJAX-Steuerelement-Toolkit-Steuerelement funktioniert genauso wie eine normale ASP.NET-Steuerelements.</span><span class="sxs-lookup"><span data-stu-id="25619-110">An AJAX Control Toolkit control works just like a normal ASP.NET control.</span></span> <span data-ttu-id="25619-111">Sie können das Steuerelement aus der Toolbox auf einer ASP.NET-Seite ziehen.</span><span class="sxs-lookup"><span data-stu-id="25619-111">You can drag the control from the toolbox onto an ASP.NET page.</span></span> <span data-ttu-id="25619-112">Sie können das Steuerelement auf der Seite in der Entwurfsansicht oder Datenquellensicht hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="25619-112">You can add the control to the page in either Design view or Source view.</span></span>

<span data-ttu-id="25619-113">Es ist eine spezielle Anforderung bei Verwendung der Steuerelemente aus dem AJAX-Steuerelement-Toolkit.</span><span class="sxs-lookup"><span data-stu-id="25619-113">There is one special requirement when using the controls from the AJAX Control Toolkit.</span></span> <span data-ttu-id="25619-114">Die Seite muss ein ScriptManager-Steuerelement enthalten.</span><span class="sxs-lookup"><span data-stu-id="25619-114">The page must contain a ScriptManager control.</span></span> <span data-ttu-id="25619-115">Die ScriptManager-Steuerelement ist verantwortlich für einschließlich sämtlicher in der die erforderlichen JavaScript-Code für die AJAX-Steuerelement-Toolkit-Steuerelemente erforderlich.</span><span class="sxs-lookup"><span data-stu-id="25619-115">The ScriptManager control is responsible for including all of the necessary JavaScript required by the AJAX Control Toolkit controls.</span></span>

<span data-ttu-id="25619-116">Die Registerkarte "AJAX Control Toolkit" enthält beispielsweise ein Steuerelement mit dem Namen der Editor-Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="25619-116">For example, the AJAX Control Toolkit tab includes a control named the Editor control.</span></span> <span data-ttu-id="25619-117">Dieses Steuerelement zeigt einen reichhaltigen HTML-Editor.</span><span class="sxs-lookup"><span data-stu-id="25619-117">This control displays a rich HTML editor.</span></span> <span data-ttu-id="25619-118">Führen Sie diese Schritte aus, um die Editor-Steuerelement zu einer Seite hinzufügen:</span><span class="sxs-lookup"><span data-stu-id="25619-118">Follow these steps to add the Editor control to a page:</span></span>

1. <span data-ttu-id="25619-119">Erstellen Sie eine neue, mit dem Namen ShowEditor.aspx ASP.NET-Seite</span><span class="sxs-lookup"><span data-stu-id="25619-119">Create a new ASP.NET page named ShowEditor.aspx</span></span>
2. <span data-ttu-id="25619-120">Wählen Sie das ScriptManager-Steuerelement vom der Registerkarte "AJAX-Erweiterungen" in der Toolbox, und ziehen Sie das Steuerelement auf der Seite.</span><span class="sxs-lookup"><span data-stu-id="25619-120">Select the ScriptManager control from beneath the AJAX Extensions tab in the toolbox and drag the control onto the page.</span></span>
3. <span data-ttu-id="25619-121">Wählen Sie die Editor-Steuerelement vom die AJAX-Steuerelement-Toolkit-Registerkarte in der Toolbox, und ziehen Sie das Steuerelement auf der Seite (siehe Abbildung 1).</span><span class="sxs-lookup"><span data-stu-id="25619-121">Select the Editor control from beneath the AJAX Control Toolkit tab in the toolbox and drag the control onto the page (see Figure 1).</span></span> <span data-ttu-id="25619-122">Der Designer sollte wie in Abbildung 2 aussehen.</span><span class="sxs-lookup"><span data-stu-id="25619-122">The Designer should look like Figure 2.</span></span>
4. <span data-ttu-id="25619-123">Führen Sie die Website im Menü die Option **Debuggen, Debugging starten** oder die F5-Taste drücken.</span><span class="sxs-lookup"><span data-stu-id="25619-123">Run the web site by selecting the menu option **Debug, Start Debugging** or hitting the F5 key.</span></span>
5. <span data-ttu-id="25619-124">Die Seite in Abbildung 3 sollte angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="25619-124">You should see the page in Figure 3.</span></span>


<span data-ttu-id="25619-125">[![Auswählen des HTML-Editor-Steuerelements](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="25619-125">[![Selecting the HTML Editor control](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image1.png)</span></span>

<span data-ttu-id="25619-126">**Abbildung 01**: Auswählen der HTML-Editor-Steuerelements ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="25619-126">**Figure 01**: Selecting the HTML Editor control([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.png))</span></span>


<span data-ttu-id="25619-127">[![Visual Studio-Designer mit ScriptManager und Edit-Steuerelement](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="25619-127">[![Visual Studio Designer with ScriptManager and Edit control](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image2.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.png)</span></span>

<span data-ttu-id="25619-128">**Abbildung 02**: Visual Studio-Designer mit ScriptManager und Edit-Steuerelement ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="25619-128">**Figure 02**: Visual Studio Designer with ScriptManager and Edit control([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.png))</span></span>


<span data-ttu-id="25619-129">[![Die Seite "DisplayEditor.aspx"](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="25619-129">[![The DisplayEditor.aspx page](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image3.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.png)</span></span>

<span data-ttu-id="25619-130">**Abbildung 03**: die DisplayEditor.aspx-Seite ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="25619-130">**Figure 03**: The DisplayEditor.aspx page([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.png))</span></span>


## <a name="using-ajax-control-toolkit-control-extenders"></a><span data-ttu-id="25619-131">AJAX Extender-Steuerelement-Toolkit-</span><span class="sxs-lookup"><span data-stu-id="25619-131">Using AJAX Control Toolkit Control Extenders</span></span>

<span data-ttu-id="25619-132">Das AJAX-Steuerelement-Toolkit enthält auch-Extender.</span><span class="sxs-lookup"><span data-stu-id="25619-132">The AJAX Control Toolkit also contains control extenders.</span></span> <span data-ttu-id="25619-133">Wie der Name andeutet, reicht ein Extendersteuerelement die Funktionalität von einem vorhandenen Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="25619-133">As its name suggests, a control extender extends the functionality of an existing control.</span></span> <span data-ttu-id="25619-134">Das Extendersteuerelement ConfirmButton erstreckt sich beispielsweise das standardmäßige ASP.NET Schaltflächen-Steuerelement aus.</span><span class="sxs-lookup"><span data-stu-id="25619-134">For example, the ConfirmButton control extender extends the standard ASP.NET Button control.</span></span> <span data-ttu-id="25619-135">Der Extender ändert das Steuerelement s Verhalten, damit die Schaltfläche ein Bestätigungsdialogfeld angezeigt, wenn Sie darauf klicken.</span><span class="sxs-lookup"><span data-stu-id="25619-135">The extender changes the Button control�s behavior so that the Button displays a confirmation dialog when you click it.</span></span>

<span data-ttu-id="25619-136">Ein Extendersteuerelement, genau wie ein AJAX-Steuerelement-Toolkit-Steuerelement erfordert ein ScriptManager-Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="25619-136">A control extender, just like an AJAX Control Toolkit control, requires a ScriptManager control.</span></span> <span data-ttu-id="25619-137">Sie müssen ein ScriptManager-Steuerelement zu einer Seite hinzufügen, bevor Sie beginnen mit-Extender auf der Seite.</span><span class="sxs-lookup"><span data-stu-id="25619-137">You must add a ScriptManager control to a page before you start using control extenders in the page.</span></span>

<span data-ttu-id="25619-138">Führen Sie diese Schritte aus, um das Extendersteuerelement ConfirmButton verwenden:</span><span class="sxs-lookup"><span data-stu-id="25619-138">Follow these steps to use the ConfirmButton control extender:</span></span>

1. <span data-ttu-id="25619-139">Erstellen Sie eine neue, mit dem Namen ShowConfirmButton.aspx ASP.NET-Seite</span><span class="sxs-lookup"><span data-stu-id="25619-139">Create a new ASP.NET page named ShowConfirmButton.aspx</span></span>
2. <span data-ttu-id="25619-140">Fügen Sie ein ScriptManager-Steuerelement durch Ziehen des Steuerelements auf der Seite vom der Registerkarte "AJAX-Erweiterungen" auf der Seite hinzu.</span><span class="sxs-lookup"><span data-stu-id="25619-140">Add a ScriptManager control to the page by dragging the control onto the page from beneath the AJAX Extensions tab.</span></span>
3. <span data-ttu-id="25619-141">Fügen Sie ein standard-Schaltflächen-Steuerelement auf der Seite durch die Schaltfläche vom die Registerkarte "Standard" in der Toolbox auf die Entwurfsoberfläche ziehen.</span><span class="sxs-lookup"><span data-stu-id="25619-141">Add a standard Button control to the page by dragging the Button from beneath the Standard tab in the toolbox onto the Designer surface.</span></span>
4. <span data-ttu-id="25619-142">Klicken Sie auf die **Extender hinzufügen** Aufgabe Option (siehe Abbildung 4).</span><span class="sxs-lookup"><span data-stu-id="25619-142">Click the **Add Extender** task option (see Figure 4).</span></span>
5. <span data-ttu-id="25619-143">Wählen Sie im Dialogfeld Extender auswählen ConfirmButtonExtender (siehe Abbildung 5), und klicken Sie auf die Schaltfläche "OK".</span><span class="sxs-lookup"><span data-stu-id="25619-143">In the Choose Extender dialog, select ConfirmButtonExtender (see Figure 5) and click the OK button.</span></span>
6. <span data-ttu-id="25619-144">Wählen Sie im Designer das Schaltflächen-Steuerelement, und erweitern Sie den Extender Button1\_ConfirmButtonExtender Knoten im Eigenschaftenfenster angezeigt (siehe Abbildung 6).</span><span class="sxs-lookup"><span data-stu-id="25619-144">Select the Button control in the Designer and expand the Extenders, Button1\_ConfirmButtonExtender node in the Properties window (see Figure 6).</span></span> <span data-ttu-id="25619-145">Weisen Sie den Wert *wirklich?* der ConfirmText-Eigenschaft.</span><span class="sxs-lookup"><span data-stu-id="25619-145">Assign the value *�Really?�* to the ConfirmText property.</span></span>
7. <span data-ttu-id="25619-146">Führen Sie die Seite, indem Sie im Menü die Option **Debuggen, Debugging starten** oder drücken Sie die F5-Taste.</span><span class="sxs-lookup"><span data-stu-id="25619-146">Run the page by selecting the menu option **Debug, Start Debugging** or hit the F5 key.</span></span>


<span data-ttu-id="25619-147">[![Die Task-Option Extender hinzufügen](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="25619-147">[![The Add Extender task option](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image4.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.png)</span></span>

<span data-ttu-id="25619-148">**Abbildung 04**: Extender für das Hinzufügen von Task-Option ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="25619-148">**Figure 04**: The Add Extender task option([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image8.png))</span></span>


<span data-ttu-id="25619-149">[![Das Extendersteuerelement ConfirmButton auswählen](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image9.png)</span><span class="sxs-lookup"><span data-stu-id="25619-149">[![Selecting the ConfirmButton control extender](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image5.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image9.png)</span></span>

<span data-ttu-id="25619-150">**Abbildung 05**: Auswählen der ConfirmButton Extendersteuerelement ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image10.png))</span><span class="sxs-lookup"><span data-stu-id="25619-150">**Figure 05**: Selecting the ConfirmButton control extender([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image10.png))</span></span>


<span data-ttu-id="25619-151">[![Festlegen einer Eigenschaft ConfirmButton](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image11.png)</span><span class="sxs-lookup"><span data-stu-id="25619-151">[![Setting a ConfirmButton property](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image6.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image11.png)</span></span>

<span data-ttu-id="25619-152">**Abbildung 06**: Festlegen einer Eigenschaft ConfirmButton ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image12.png))</span><span class="sxs-lookup"><span data-stu-id="25619-152">**Figure 06**: Setting a ConfirmButton property([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image12.png))</span></span>


<span data-ttu-id="25619-153">Wenn die Seite geöffnet wird, sollte eine Schaltfläche angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="25619-153">When the page opens, you should see a button.</span></span> <span data-ttu-id="25619-154">Wenn Sie die Schaltfläche klicken, erhalten Sie im Bestätigungsdialogfeld in Abbildung 7.</span><span class="sxs-lookup"><span data-stu-id="25619-154">When you click the button, you get the confirmation dialog in Figure 7.</span></span>


<span data-ttu-id="25619-155">[![Bestätigungsdialogfeld anzeigen](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image13.png)</span><span class="sxs-lookup"><span data-stu-id="25619-155">[![Displaying the confirmation dialog](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image7.jpg)](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image13.png)</span></span>

<span data-ttu-id="25619-156">**Abbildung 07**: Anzeigen von Bestätigungsdialogfeld ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image14.png))</span><span class="sxs-lookup"><span data-stu-id="25619-156">**Figure 07**: Displaying the confirmation dialog([Click to view full-size image](using-ajax-control-toolkit-controls-and-control-extenders-vb/_static/image14.png))</span></span>


<span data-ttu-id="25619-157">Beachten Sie, dass Sie nicht normalerweise ein Extendersteuerelement auf einer Seite ziehen.</span><span class="sxs-lookup"><span data-stu-id="25619-157">Notice that you normally do not drag a control extender onto a page.</span></span> <span data-ttu-id="25619-158">Verwenden Sie stattdessen die **Extender hinzufügen** Aufgabe Option aus, um einen Extender an ein Steuerelement hinzufügen, die Sie bereits zu einer Seite hinzugefügt haben.</span><span class="sxs-lookup"><span data-stu-id="25619-158">Instead, you use the **Add Extender** task option to add an extender to a control that you have already added to a page.</span></span> <span data-ttu-id="25619-159">Beachten Sie außerdem, dass Sie Steuerelement Extendereigenschaften durch Öffnen der Eigenschaftenseite für das Steuerelement zu vergrößernden festzulegen.</span><span class="sxs-lookup"><span data-stu-id="25619-159">Notice, furthermore, that you set control extender properties by opening the property sheet for the control being extended.</span></span>

<span data-ttu-id="25619-160">Eine einzelne ASP.NET-Steuerelements kann von mehreren-Extender erweitert werden.</span><span class="sxs-lookup"><span data-stu-id="25619-160">A single ASP.NET control can be extended by multiple control extenders.</span></span> <span data-ttu-id="25619-161">Das Eigenschaftenblatt für das Steuerelement zu vergrößernden Listet alle Extender Steuerelement dem Steuerelement zugeordnet.</span><span class="sxs-lookup"><span data-stu-id="25619-161">The property sheet for the control being extended will list all of the control extenders associated with the control.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="25619-162">[Zurück](get-started-with-the-ajax-control-toolkit-vb.md)
> [Weiter](creating-a-custom-ajax-control-toolkit-control-extender-vb.md)</span><span class="sxs-lookup"><span data-stu-id="25619-162">[Previous](get-started-with-the-ajax-control-toolkit-vb.md)
[Next](creating-a-custom-ajax-control-toolkit-control-extender-vb.md)</span></span>
