---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
title: Hinzufügen von Animationen zu einem Steuerelement (c#) | Microsoft-Dokumentation
author: wenz
description: Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Dieses Tutorial zeigt, wie...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 0f1fc1f5-9dbd-44e7-931e-387d42f0342b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-cs
msc.type: authoredcontent
ms.openlocfilehash: aa977883af931bb74b791104cf4ee3212079e43a
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833943"
---
<a name="adding-animation-to-a-control-c"></a><span data-ttu-id="f5908-104">Hinzufügen von Animationen zu einem Steuerelement (c#)</span><span class="sxs-lookup"><span data-stu-id="f5908-104">Adding Animation to a Control (C#)</span></span>
====================
<span data-ttu-id="f5908-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="f5908-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="f5908-106">[Code herunterladen](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.cs.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="f5908-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1CS.pdf)</span></span>

> <span data-ttu-id="f5908-107">Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="f5908-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="f5908-108">In diesem Tutorial zeigt, wie Sie eine solche Animation einrichten.</span><span class="sxs-lookup"><span data-stu-id="f5908-108">This tutorial shows how to set up such an animation.</span></span>


## <a name="overview"></a><span data-ttu-id="f5908-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="f5908-109">Overview</span></span>

<span data-ttu-id="f5908-110">Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="f5908-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="f5908-111">In diesem Tutorial zeigt, wie Sie eine solche Animation einrichten.</span><span class="sxs-lookup"><span data-stu-id="f5908-111">This tutorial shows how to set up such an animation.</span></span>

## <a name="steps"></a><span data-ttu-id="f5908-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="f5908-112">Steps</span></span>

<span data-ttu-id="f5908-113">Der erste Schritt ist wie üblich, enthalten die `ScriptManager` auf der Seite, damit die ASP.NET AJAX-Bibliothek geladen wird und das Steuerelement-Toolkit verwendet werden kann:</span><span class="sxs-lookup"><span data-stu-id="f5908-113">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample1.aspx)]

<span data-ttu-id="f5908-114">Die Animation in diesem Szenario wird auf einen Bereich des Texts angewendet werden, der so aussieht:</span><span class="sxs-lookup"><span data-stu-id="f5908-114">The animation in this scenario will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample2.aspx)]

<span data-ttu-id="f5908-115">Die zugeordnete CSS-Klasse für den Bereich definiert eine Hintergrundfarbe und eine Breite:</span><span class="sxs-lookup"><span data-stu-id="f5908-115">The associated CSS class for the panel defines a background color and a width:</span></span>

[!code-css[Main](adding-animation-to-a-control-cs/samples/sample3.css)]

<span data-ttu-id="f5908-116">Als Nächstes eingerichtet ist, müssen wir die `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="f5908-116">Next up, we need the `AnimationExtender`.</span></span> <span data-ttu-id="f5908-117">Nach der Bereitstellung einer `ID` und die üblichen `runat="server"`, `TargetControlID` Attribut muss auf das Steuerelement zum Animieren in unserem Fall ist der Bereich festgelegt werden:</span><span class="sxs-lookup"><span data-stu-id="f5908-117">After providing an `ID` and the usual `runat="server"`, the `TargetControlID` attribute must be set to the control to animate in our case, the panel:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample4.aspx)]

<span data-ttu-id="f5908-118">Die gesamte deklarativ animiert werden mithilfe einer XML-Syntax, die leider zurzeit nicht vollständig von Visual Studio IntelliSense unterstützt.</span><span class="sxs-lookup"><span data-stu-id="f5908-118">The whole animation is applied declaratively, using an XML syntax, unfortunately currently not fully supported by Visual Studio's IntelliSense.</span></span> <span data-ttu-id="f5908-119">Der Stammknoten ist `<Animations>;` innerhalb des Knotens, sind mehrere Ereignisse zulässig, die zu bestimmen, wann die Animation(s) Ort take(s):</span><span class="sxs-lookup"><span data-stu-id="f5908-119">The root node is `<Animations>;` within this node, several events are allowed which determine when the animation(s) take(s) place:</span></span>

- <span data-ttu-id="f5908-120">`OnClick` (Mausklick)</span><span class="sxs-lookup"><span data-stu-id="f5908-120">`OnClick` (mouse click)</span></span>
- <span data-ttu-id="f5908-121">`OnHoverOut` (wenn die Maus für ein Steuerelement verlässt)</span><span class="sxs-lookup"><span data-stu-id="f5908-121">`OnHoverOut` (when the mouse leaves a control)</span></span>
- <span data-ttu-id="f5908-122">`OnHoverOver` (wenn die Maus über ein Steuerelement bewegt wird, beenden die `OnHoverOut` Animation)</span><span class="sxs-lookup"><span data-stu-id="f5908-122">`OnHoverOver` (when the mouse hovers over a control, stopping the `OnHoverOut` animation)</span></span>
- <span data-ttu-id="f5908-123">`OnLoad` (wenn die Seite geladen wurde)</span><span class="sxs-lookup"><span data-stu-id="f5908-123">`OnLoad` (when the page has been loaded)</span></span>
- <span data-ttu-id="f5908-124">`OnMouseOut` (wenn die Maus für ein Steuerelement verlässt)</span><span class="sxs-lookup"><span data-stu-id="f5908-124">`OnMouseOut` (when the mouse leaves a control)</span></span>
- <span data-ttu-id="f5908-125">`OnMouseOver` (wenn die Maus über ein Steuerelement bewegt wird, beenden Sie nicht die `OnMouseOut` Animation)</span><span class="sxs-lookup"><span data-stu-id="f5908-125">`OnMouseOver` (when the mouse hovers over a control, not stopping the `OnMouseOut` animation)</span></span>

<span data-ttu-id="f5908-126">Das Framework bietet einen Satz von Animationen, jeweils durch eine eigene XML-Element dargestellt wird.</span><span class="sxs-lookup"><span data-stu-id="f5908-126">The framework comes with a set of animations, each one represented by its own XML element.</span></span> <span data-ttu-id="f5908-127">Hier ist eine Auswahl aus:</span><span class="sxs-lookup"><span data-stu-id="f5908-127">Here is a selection:</span></span>

- <span data-ttu-id="f5908-128">`<Color>` (ändern eine Farbe)</span><span class="sxs-lookup"><span data-stu-id="f5908-128">`<Color>` (changing a color)</span></span>
- <span data-ttu-id="f5908-129">`<FadeIn>` (in ausblenden)</span><span class="sxs-lookup"><span data-stu-id="f5908-129">`<FadeIn>` (fading in)</span></span>
- <span data-ttu-id="f5908-130">`<FadeOut>` (ausblenden)</span><span class="sxs-lookup"><span data-stu-id="f5908-130">`<FadeOut>` (fading out)</span></span>
- <span data-ttu-id="f5908-131">`<Property>` (ändern die Eigenschaft eines Steuerelements)</span><span class="sxs-lookup"><span data-stu-id="f5908-131">`<Property>` (changing a control's property)</span></span>
- <span data-ttu-id="f5908-132">`<Pulse>` (pulsating)</span><span class="sxs-lookup"><span data-stu-id="f5908-132">`<Pulse>` (pulsating)</span></span>
- <span data-ttu-id="f5908-133">`<Resize>` (Ändern der Größe)</span><span class="sxs-lookup"><span data-stu-id="f5908-133">`<Resize>` (changing the size)</span></span>
- <span data-ttu-id="f5908-134">`<Scale>` (proportional die Größe ändern)</span><span class="sxs-lookup"><span data-stu-id="f5908-134">`<Scale>` (proportionally changing the size)</span></span>

<span data-ttu-id="f5908-135">In diesem Beispiel wird der Bereich ausblenden. Die Animation treffen 1,5 Sekunden (`Duration` Attribut), Anzeige (Animationsschritte) zur 24 Bildern pro Sekunde (`Fps` Attributs).</span><span class="sxs-lookup"><span data-stu-id="f5908-135">In this example, the panel shall fade out. The animation shall take 1.5 seconds (`Duration` attribute), displaying 24 frames (animation steps) per second (`Fps` attributs).</span></span> <span data-ttu-id="f5908-136">Hier ist das vollständige Markup für die `AnimationExtender` Steuerelement:</span><span class="sxs-lookup"><span data-stu-id="f5908-136">Here is the complete markup for the `AnimationExtender` control:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-cs/samples/sample5.aspx)]

<span data-ttu-id="f5908-137">Wenn Sie dieses Skript ausführen, wird der Bereich wird angezeigt, und in 1,5 Sekunden ausgeblendet wird.</span><span class="sxs-lookup"><span data-stu-id="f5908-137">When you run this script, the panel is displayed and fades out in one and a half seconds.</span></span>


<span data-ttu-id="f5908-138">[![Der Bereich wird ausgeblendet wird](adding-animation-to-a-control-cs/_static/image2.png)](adding-animation-to-a-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="f5908-138">[![The panel is fading out](adding-animation-to-a-control-cs/_static/image2.png)](adding-animation-to-a-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="f5908-139">Der Bereich wird ausgeblendet wird, ([klicken Sie, um das Bild in voller Größe anzeigen](adding-animation-to-a-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="f5908-139">The panel is fading out ([Click to view full-size image](adding-animation-to-a-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="f5908-140">Nächste</span><span class="sxs-lookup"><span data-stu-id="f5908-140">Next</span></span>](executing-several-animations-at-the-same-time-cs.md)
