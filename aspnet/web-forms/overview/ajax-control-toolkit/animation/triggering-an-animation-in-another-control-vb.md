---
uid: web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
title: Auslösen einer Animation in einem anderen Steuerelement (VB) | Microsoft-Dokumentation
author: wenz
description: Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Starten Sie in der Regel ein...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 25ebaf1f-5a9f-423d-98c7-1d694e93664f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/triggering-an-animation-in-another-control-vb
msc.type: authoredcontent
ms.openlocfilehash: d9ef7d35e34bd4f2b1433f7fb02d0c48834b2357
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804891"
---
<a name="triggering-an-animation-in-another-control-vb"></a><span data-ttu-id="2ad78-104">Auslösen einer Animation in einem anderen Steuerelement (VB)</span><span class="sxs-lookup"><span data-stu-id="2ad78-104">Triggering an Animation in another Control (VB)</span></span>
====================
<span data-ttu-id="2ad78-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="2ad78-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="2ad78-106">[Code herunterladen](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.vb.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="2ad78-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation8.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation8VB.pdf)</span></span>

> <span data-ttu-id="2ad78-107">Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="2ad78-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="2ad78-108">Starten eine Animation wird im Allgemeinen durch eine Benutzerinteraktion mit dem gleichen Steuerelement ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="2ad78-108">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="2ad78-109">Es ist jedoch auch möglich, ein Steuerelement, und klicken Sie dann Animation ein anderes Steuerelement interagieren.</span><span class="sxs-lookup"><span data-stu-id="2ad78-109">It is however also possible to interact with one control and then animation another control.</span></span>


## <a name="overview"></a><span data-ttu-id="2ad78-110">Übersicht</span><span class="sxs-lookup"><span data-stu-id="2ad78-110">Overview</span></span>

<span data-ttu-id="2ad78-111">Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="2ad78-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="2ad78-112">Starten eine Animation wird im Allgemeinen durch eine Benutzerinteraktion mit dem gleichen Steuerelement ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="2ad78-112">Generally, launching an animation is triggered by user interaction with the same control.</span></span> <span data-ttu-id="2ad78-113">Es ist jedoch auch möglich, ein Steuerelement, und klicken Sie dann Animation ein anderes Steuerelement interagieren.</span><span class="sxs-lookup"><span data-stu-id="2ad78-113">It is however also possible to interact with one control and then animation another control.</span></span>

## <a name="steps"></a><span data-ttu-id="2ad78-114">Schritte</span><span class="sxs-lookup"><span data-stu-id="2ad78-114">Steps</span></span>

<span data-ttu-id="2ad78-115">Zunächst einmal sind die `ScriptManager` in die Seite klicken Sie dann die ASP.NET AJAX-Bibliothek wird geladen, lässt sich das Steuerelement-Toolkit verwenden:</span><span class="sxs-lookup"><span data-stu-id="2ad78-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample1.aspx)]

<span data-ttu-id="2ad78-116">Die Animation wird auf einen Bereich des Texts angewendet werden, der so aussieht:</span><span class="sxs-lookup"><span data-stu-id="2ad78-116">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample2.aspx)]

<span data-ttu-id="2ad78-117">Definieren Sie in der zugehörigen CSS-Klasse für den Bereich eine gute Hintergrundfarbe aus, und auch festlegen Sie eine feste Breite für den Bereich:</span><span class="sxs-lookup"><span data-stu-id="2ad78-117">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](triggering-an-animation-in-another-control-vb/samples/sample3.css)]

<span data-ttu-id="2ad78-118">Um das Panel Animation zu starten, wird eine HTML-Schaltfläche verwendet.</span><span class="sxs-lookup"><span data-stu-id="2ad78-118">In order to start animating the panel, an HTML button is used.</span></span> <span data-ttu-id="2ad78-119">Beachten Sie, dass `<input type="button" />` ist über begünstigt `<asp:Button />` da wir einen Postback nicht soll, wenn der Benutzer auf diese Schaltfläche klickt.</span><span class="sxs-lookup"><span data-stu-id="2ad78-119">Note that `<input type="button" />` is favoured over `<asp:Button />` since we do not want a postback when the user clicks on that button.</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample4.aspx)]

<span data-ttu-id="2ad78-120">Fügen Sie dann die `AnimationExtender` auf der Seite Bereitstellen einer `ID`, `TargetControlID` -Attribut und das obligatorische `runat="server"`.</span><span class="sxs-lookup"><span data-stu-id="2ad78-120">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`.</span></span> <span data-ttu-id="2ad78-121">Es ist wichtig, legen Sie `TargetControlID` auf die ID der Schaltfläche (das Auslösen der Animation-Element), nicht auf die ID des Bereichs (Element der zu animierenden)</span><span class="sxs-lookup"><span data-stu-id="2ad78-121">It is important to set `TargetControlID` to the ID of the button (the element triggering the animation), not to the ID of the panel (the element being animated)</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample5.aspx)]

<span data-ttu-id="2ad78-122">In der `<Animations>` Knoten Ort Animationen wie gewohnt.</span><span class="sxs-lookup"><span data-stu-id="2ad78-122">Within the `<Animations>` node, place animations as usual.</span></span> <span data-ttu-id="2ad78-123">Damit sie im Bereich geändert werden können, legen Sie nicht die Schaltfläche, die `AnimationTarget` -Attribut für jedes Animationselement in `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="2ad78-123">In order to make them change the panel, not the button, set the `AnimationTarget` attribute for every animation element within `AnimationExtender`.</span></span> <span data-ttu-id="2ad78-124">Der Wert für `AnimationTarget` ist die ID des Bereichs, natürlich.</span><span class="sxs-lookup"><span data-stu-id="2ad78-124">The value for `AnimationTarget` is the ID of the panel, of course.</span></span> <span data-ttu-id="2ad78-125">Auf diese Weise Fall die Animationen im Bereich nicht mit der Schaltfläche mit den auslösenden sein.</span><span class="sxs-lookup"><span data-stu-id="2ad78-125">That way, the animations happen with the panel, not with the triggering button.</span></span> <span data-ttu-id="2ad78-126">Hier ist die `AnimationExtender` Markup für dieses Szenario:</span><span class="sxs-lookup"><span data-stu-id="2ad78-126">Here is the `AnimationExtender` markup for this scenario:</span></span>

[!code-aspx[Main](triggering-an-animation-in-another-control-vb/samples/sample6.aspx)]

<span data-ttu-id="2ad78-127">Beachten Sie die spezielle Reihenfolge, in der die einzelnen Animationen angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="2ad78-127">Note the special order in which the individual animations appear.</span></span> <span data-ttu-id="2ad78-128">Als Erstes ruft die Schaltfläche deaktiviert, nachdem die Animation ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="2ad78-128">First of all, the button gets deactivated once the animation runs.</span></span> <span data-ttu-id="2ad78-129">Da gibt es keine `AnimationTarget` -Attribut in der `<EnableAction>` Element dieser Animation wird angewendet, auf das ursprüngliche Steuerelement: die Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="2ad78-129">Since there is no `AnimationTarget` attribute in the `<EnableAction>` element, this animation is applied to the originating control: the button.</span></span> <span data-ttu-id="2ad78-130">Die Animationsschritte für die nächsten beiden parallelly erfolgt (`<Parallel>` Element).</span><span class="sxs-lookup"><span data-stu-id="2ad78-130">The next two animation steps shall be carried out parallelly (`<Parallel>` element).</span></span> <span data-ttu-id="2ad78-131">Beide verfügen über ihre `AnimationTarget` Attribute festgelegt werden, um `"Panel1"`, animieren daher im Bereich nicht auf die Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="2ad78-131">Both have their `AnimationTarget` attributes set to `"Panel1"`, thus animating the panel, not the button.</span></span>


<span data-ttu-id="2ad78-132">[![Startet die Animation Bereich, klicken mit der Maus auf die Schaltfläche](triggering-an-animation-in-another-control-vb/_static/image2.png)](triggering-an-animation-in-another-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2ad78-132">[![A mouse click on the button starts the panel animation](triggering-an-animation-in-another-control-vb/_static/image2.png)](triggering-an-animation-in-another-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="2ad78-133">Startet die Animation Bereich, klicken mit der Maus auf die Schaltfläche ([klicken Sie, um das Bild in voller Größe anzeigen](triggering-an-animation-in-another-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="2ad78-133">A mouse click on the button starts the panel animation ([Click to view full-size image](triggering-an-animation-in-another-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2ad78-134">[Zurück](disabling-actions-during-animation-vb.md)
> [Weiter](modifying-animations-from-the-server-side-vb.md)</span><span class="sxs-lookup"><span data-stu-id="2ad78-134">[Previous](disabling-actions-during-animation-vb.md)
[Next](modifying-animations-from-the-server-side-vb.md)</span></span>
