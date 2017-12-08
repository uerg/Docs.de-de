---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
title: "Auslösen einer Animation in einem anderen Steuerelement (VB) | Microsoft Docs"
author: wenz
description: "Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. Starten Sie in der Regel ein..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 25ebaf1f-5a9f-423d-98c7-1d694e93664f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
msc.type: authoredcontent
ms.openlocfilehash: ce1d29cbd06ef8a470780ff4c7bda8039575d59f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="triggering-an-animation-in-another-control-vb"></a><span data-ttu-id="dd20a-104">Auslösen einer Animation in einem anderen Steuerelement (VB)</span><span class="sxs-lookup"><span data-stu-id="dd20a-104">Triggering an Animation in another Control (VB)</span></span>
====================
<span data-ttu-id="dd20a-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="dd20a-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="dd20a-106">[Herunterladen von Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.vb.zip) oder [PDF herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="dd20a-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8VB.pdf)</span></span>

> <span data-ttu-id="dd20a-107">Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="dd20a-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="dd20a-108">Starten eine Animation wird im Allgemeinen durch eine Benutzerinteraktion mit dem gleichen Steuerelement ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="dd20a-108">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="dd20a-109">Es ist jedoch auch möglich, mit einem Steuerelement, und klicken Sie dann Animation ein anderes Steuerelement interagieren.</span><span class="sxs-lookup"><span data-stu-id="dd20a-109">It is however also possible to interact with one control and then animation another control.</span></span>


## <a name="overview"></a><span data-ttu-id="dd20a-110">Übersicht</span><span class="sxs-lookup"><span data-stu-id="dd20a-110">Overview</span></span>

<span data-ttu-id="dd20a-111">Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="dd20a-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="dd20a-112">Starten eine Animation wird im Allgemeinen durch eine Benutzerinteraktion mit dem gleichen Steuerelement ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="dd20a-112">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="dd20a-113">Es ist jedoch auch möglich, mit einem Steuerelement, und klicken Sie dann Animation ein anderes Steuerelement interagieren.</span><span class="sxs-lookup"><span data-stu-id="dd20a-113">It is however also possible to interact with one control and then animation another control.</span></span>

## <a name="steps"></a><span data-ttu-id="dd20a-114">Schritte</span><span class="sxs-lookup"><span data-stu-id="dd20a-114">Steps</span></span>

<span data-ttu-id="dd20a-115">Erstens sind die `ScriptManager` auf der Seite; klicken Sie dann die ASP.NET AJAX-Bibliothek geladen ist, wodurch das Steuerelement-Toolkit verwenden:</span><span class="sxs-lookup"><span data-stu-id="dd20a-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample1.aspx)]

<span data-ttu-id="dd20a-116">Die Animation wird auf einen Bereich des Texts angewendet werden, die wie folgt aussieht:</span><span class="sxs-lookup"><span data-stu-id="dd20a-116">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample2.aspx)]

<span data-ttu-id="dd20a-117">Definieren Sie in der zugehörigen CSS-Klasse für den Bereich eine gute Hintergrundfarbe und auch festlegen Sie eine feste Breite für den Bereich:</span><span class="sxs-lookup"><span data-stu-id="dd20a-117">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](triggering-an-animation-in-another-control-vb/samples/sample3.css)]

<span data-ttu-id="dd20a-118">Um das Panel Animation starten, wird eine HTML-Schaltfläche verwendet.</span><span class="sxs-lookup"><span data-stu-id="dd20a-118">In order to start animating the panel, an HTML button is used.</span></span> <span data-ttu-id="dd20a-119">Beachten Sie, dass `<input type="button" />` ist über begünstigt `<asp:Button />` da wir einen Postback nicht soll, wenn der Benutzer auf die Schaltfläche klickt.</span><span class="sxs-lookup"><span data-stu-id="dd20a-119">Note that `<input type="button" />` is favoured over `<asp:Button />` since we do not want a postback when the user clicks on that button.</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample4.aspx)]

<span data-ttu-id="dd20a-120">Fügen Sie dann die `AnimationExtender` auf der Seite "Bereitstellen einer `ID`, die `TargetControlID` Attribut und der Auswahlparameter `runat="server"`.</span><span class="sxs-lookup"><span data-stu-id="dd20a-120">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`.</span></span> <span data-ttu-id="dd20a-121">Es ist wichtig, legen Sie `TargetControlID` auf die ID der Schaltfläche (die die Animation auslösen-Element), nicht auf die ID des Bereichs (Element der zu animierenden)</span><span class="sxs-lookup"><span data-stu-id="dd20a-121">It is important to set `TargetControlID` to the ID of the button (the element triggering the animation), not to the ID of the panel (the element being animated)</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample5.aspx)]

<span data-ttu-id="dd20a-122">Innerhalb der `<Animations>` Knoten Stelle Animationen wie gewohnt.</span><span class="sxs-lookup"><span data-stu-id="dd20a-122">Within the `<Animations>` node, place animations as usual.</span></span> <span data-ttu-id="dd20a-123">Um den Bereich ändern machen, nicht die Schaltfläche ist die `AnimationTarget` -Attributs für jedes Animationselement in `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="dd20a-123">In order to make them change the panel, not the button, set the `AnimationTarget` attribute for every animation element within `AnimationExtender`.</span></span> <span data-ttu-id="dd20a-124">Der Wert für `AnimationTarget` ist die ID des Bereichs, natürlich.</span><span class="sxs-lookup"><span data-stu-id="dd20a-124">The value for `AnimationTarget` is the ID of the panel, of course.</span></span> <span data-ttu-id="dd20a-125">Auf diese Weise geschehen Animationen mit im Bereich nicht mit der auslösenden Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="dd20a-125">That way, the animations happen with the panel, not with the triggering button.</span></span> <span data-ttu-id="dd20a-126">So sieht die `AnimationExtender` Markup für dieses Szenario:</span><span class="sxs-lookup"><span data-stu-id="dd20a-126">Here is the `AnimationExtender` markup for this scenario:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample6.aspx)]

<span data-ttu-id="dd20a-127">Beachten Sie die spezielle Reihenfolge, in der die einzelnen Animationen angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="dd20a-127">Note the special order in which the individual animations appear.</span></span> <span data-ttu-id="dd20a-128">Erstens Ruft die Schaltfläche deaktiviert, sobald die Animation ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="dd20a-128">First of all, the button gets deactivated once the animation runs.</span></span> <span data-ttu-id="dd20a-129">Pflege, weil keine `AnimationTarget` Attribut in der `<EnableAction>` Element dieser Animation auf die ursprünglichen Steuerelement angewendet wird: die Schaltfläche "".</span><span class="sxs-lookup"><span data-stu-id="dd20a-129">Since there is no `AnimationTarget` attribute in the `<EnableAction>` element, this animation is applied to the originating control: the button.</span></span> <span data-ttu-id="dd20a-130">Die nächsten beiden Animationsschritte erfolgt parallelly (`<Parallel>` Element).</span><span class="sxs-lookup"><span data-stu-id="dd20a-130">The next two animation steps shall be carried out parallelly (`<Parallel>` element).</span></span> <span data-ttu-id="dd20a-131">Beide verfügen über ihre `AnimationTarget` Attribute festgelegt werden, um `"Panel1"`, daher Animieren der Bereich nicht auf die Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="dd20a-131">Both have their `AnimationTarget` attributes set to `"Panel1"`, thus animating the panel, not the button.</span></span>


<span data-ttu-id="dd20a-132">[![Klicken mit der Maus auf die Schaltfläche "" startet das Panel animation](triggering-an-animation-in-another-control-vb/_static/image2.png)](triggering-an-animation-in-another-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="dd20a-132">[![A mouse click on the button starts the panel animation](triggering-an-animation-in-another-control-vb/_static/image2.png)](triggering-an-animation-in-another-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="dd20a-133">Startet die Panel Animation, klicken mit der Maus auf die Schaltfläche ([klicken Sie hier, um das Bild in voller Größe angezeigt](triggering-an-animation-in-another-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="dd20a-133">A mouse click on the button starts the panel animation ([Click to view full-size image](triggering-an-animation-in-another-control-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="dd20a-134">[Zurück](disabling-actions-during-animation-vb.md)
[Weiter](modifying-animations-from-the-server-side-vb.md)</span><span class="sxs-lookup"><span data-stu-id="dd20a-134">[Previous](disabling-actions-during-animation-vb.md)
[Next](modifying-animations-from-the-server-side-vb.md)</span></span>
