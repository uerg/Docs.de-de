---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
title: "Auslösen einer Animation in ein anderes Steuerelement (c#) | Microsoft Docs"
author: wenz
description: "Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. Starten Sie in der Regel ein..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e5d99c2b-d8ee-413c-80d5-c120cffb0a4c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 8d243eebc42b66f1e86b38a1b7531e527144ea7e
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="triggering-an-animation-in-another-control-c"></a><span data-ttu-id="dfb6c-104">Auslösen einer Animation in ein anderes Steuerelement (c#)</span><span class="sxs-lookup"><span data-stu-id="dfb6c-104">Triggering an Animation in another Control (C#)</span></span>
====================
<span data-ttu-id="dfb6c-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="dfb6c-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="dfb6c-106">[Herunterladen von Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.cs.zip) oder [PDF herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="dfb6c-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8CS.pdf)</span></span>

> <span data-ttu-id="dfb6c-107">Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="dfb6c-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="dfb6c-108">Starten eine Animation wird im Allgemeinen durch eine Benutzerinteraktion mit dem gleichen Steuerelement ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="dfb6c-108">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="dfb6c-109">Es ist jedoch auch möglich, mit einem Steuerelement, und klicken Sie dann Animation ein anderes Steuerelement interagieren.</span><span class="sxs-lookup"><span data-stu-id="dfb6c-109">It is however also possible to interact with one control and then animation another control.</span></span>


## <a name="overview"></a><span data-ttu-id="dfb6c-110">Übersicht</span><span class="sxs-lookup"><span data-stu-id="dfb6c-110">Overview</span></span>

<span data-ttu-id="dfb6c-111">Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="dfb6c-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="dfb6c-112">Starten eine Animation wird im Allgemeinen durch eine Benutzerinteraktion mit dem gleichen Steuerelement ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="dfb6c-112">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="dfb6c-113">Es ist jedoch auch möglich, mit einem Steuerelement, und klicken Sie dann Animation ein anderes Steuerelement interagieren.</span><span class="sxs-lookup"><span data-stu-id="dfb6c-113">It is however also possible to interact with one control and then animation another control.</span></span>

## <a name="steps"></a><span data-ttu-id="dfb6c-114">Schritte</span><span class="sxs-lookup"><span data-stu-id="dfb6c-114">Steps</span></span>

<span data-ttu-id="dfb6c-115">Erstens sind die `ScriptManager` auf der Seite; klicken Sie dann die ASP.NET AJAX-Bibliothek geladen ist, wodurch das Steuerelement-Toolkit verwenden:</span><span class="sxs-lookup"><span data-stu-id="dfb6c-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample1.aspx)]

<span data-ttu-id="dfb6c-116">Die Animation wird auf einen Bereich des Texts angewendet werden, die wie folgt aussieht:</span><span class="sxs-lookup"><span data-stu-id="dfb6c-116">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample2.aspx)]

<span data-ttu-id="dfb6c-117">Definieren Sie in der zugehörigen CSS-Klasse für den Bereich eine gute Hintergrundfarbe und auch festlegen Sie eine feste Breite für den Bereich:</span><span class="sxs-lookup"><span data-stu-id="dfb6c-117">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](triggering-an-animation-in-another-control-cs/samples/sample3.css)]

<span data-ttu-id="dfb6c-118">Um das Panel Animation starten, wird eine HTML-Schaltfläche verwendet.</span><span class="sxs-lookup"><span data-stu-id="dfb6c-118">In order to start animating the panel, an HTML button is used.</span></span> <span data-ttu-id="dfb6c-119">Beachten Sie, dass `<input type="button" />` ist über begünstigt `<asp:Button />` da wir einen Postback nicht soll, wenn der Benutzer auf die Schaltfläche klickt.</span><span class="sxs-lookup"><span data-stu-id="dfb6c-119">Note that `<input type="button" />` is favoured over `<asp:Button />` since we do not want a postback when the user clicks on that button.</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample4.aspx)]

<span data-ttu-id="dfb6c-120">Fügen Sie dann die `AnimationExtender` auf der Seite "Bereitstellen einer `ID`, die `TargetControlID` Attribut und der Auswahlparameter `runat="server"`.</span><span class="sxs-lookup"><span data-stu-id="dfb6c-120">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`.</span></span> <span data-ttu-id="dfb6c-121">Es ist wichtig, legen Sie `TargetControlID` auf die ID der Schaltfläche (die die Animation auslösen-Element), nicht auf die ID des Bereichs (Element der zu animierenden)</span><span class="sxs-lookup"><span data-stu-id="dfb6c-121">It is important to set `TargetControlID` to the ID of the button (the element triggering the animation), not to the ID of the panel (the element being animated)</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample5.aspx)]

<span data-ttu-id="dfb6c-122">Innerhalb der `<Animations>` Knoten Stelle Animationen wie gewohnt.</span><span class="sxs-lookup"><span data-stu-id="dfb6c-122">Within the `<Animations>` node, place animations as usual.</span></span> <span data-ttu-id="dfb6c-123">Um den Bereich ändern machen, nicht die Schaltfläche ist die `AnimationTarget` -Attributs für jedes Animationselement in `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="dfb6c-123">In order to make them change the panel, not the button, set the `AnimationTarget` attribute for every animation element within `AnimationExtender`.</span></span> <span data-ttu-id="dfb6c-124">Der Wert für `AnimationTarget` ist die ID des Bereichs, natürlich.</span><span class="sxs-lookup"><span data-stu-id="dfb6c-124">The value for `AnimationTarget` is the ID of the panel, of course.</span></span> <span data-ttu-id="dfb6c-125">Auf diese Weise geschehen Animationen mit im Bereich nicht mit der auslösenden Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="dfb6c-125">That way, the animations happen with the panel, not with the triggering button.</span></span> <span data-ttu-id="dfb6c-126">So sieht die `AnimationExtender` Markup für dieses Szenario:</span><span class="sxs-lookup"><span data-stu-id="dfb6c-126">Here is the `AnimationExtender` markup for this scenario:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-cs/samples/sample6.aspx)]

<span data-ttu-id="dfb6c-127">Beachten Sie die spezielle Reihenfolge, in der die einzelnen Animationen angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="dfb6c-127">Note the special order in which the individual animations appear.</span></span> <span data-ttu-id="dfb6c-128">Erstens Ruft die Schaltfläche deaktiviert, sobald die Animation ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="dfb6c-128">First of all, the button gets deactivated once the animation runs.</span></span> <span data-ttu-id="dfb6c-129">Pflege, weil keine `AnimationTarget` Attribut in der `<EnableAction>` Element dieser Animation auf die ursprünglichen Steuerelement angewendet wird: die Schaltfläche "".</span><span class="sxs-lookup"><span data-stu-id="dfb6c-129">Since there is no `AnimationTarget` attribute in the `<EnableAction>` element, this animation is applied to the originating control: the button.</span></span> <span data-ttu-id="dfb6c-130">Die nächsten beiden Animationsschritte erfolgt parallelly (`<Parallel>` Element).</span><span class="sxs-lookup"><span data-stu-id="dfb6c-130">The next two animation steps shall be carried out parallelly (`<Parallel>` element).</span></span> <span data-ttu-id="dfb6c-131">Beide verfügen über ihre `AnimationTarget` Attribute festgelegt werden, um `"Panel1"`, daher Animieren der Bereich nicht auf die Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="dfb6c-131">Both have their `AnimationTarget` attributes set to `"Panel1"`, thus animating the panel, not the button.</span></span>


<span data-ttu-id="dfb6c-132">[![Klicken mit der Maus auf die Schaltfläche "" startet das Panel animation](triggering-an-animation-in-another-control-cs/_static/image2.png)](triggering-an-animation-in-another-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="dfb6c-132">[![A mouse click on the button starts the panel animation](triggering-an-animation-in-another-control-cs/_static/image2.png)](triggering-an-animation-in-another-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="dfb6c-133">Startet die Panel Animation, klicken mit der Maus auf die Schaltfläche ([klicken Sie hier, um das Bild in voller Größe angezeigt](triggering-an-animation-in-another-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="dfb6c-133">A mouse click on the button starts the panel animation ([Click to view full-size image](triggering-an-animation-in-another-control-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="dfb6c-134">[Zurück](disabling-actions-during-animation-cs.md)
[Weiter](modifying-animations-from-the-server-side-cs.md)</span><span class="sxs-lookup"><span data-stu-id="dfb6c-134">[Previous](disabling-actions-during-animation-cs.md)
[Next](modifying-animations-from-the-server-side-cs.md)</span></span>
