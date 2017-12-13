---
uid: web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
title: "Hinzufügen von Animationen zu einem Steuerelement (VB) | Microsoft Docs"
author: wenz
description: "Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. In diesem Lernprogramm wird gezeigt, wie..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: c120187e-963e-4439-bb85-32771bc7f1f4
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/adding-animation-to-a-control-vb
msc.type: authoredcontent
ms.openlocfilehash: c2d6971ade89405245c8d23cafb6fd8bb9468639
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="adding-animation-to-a-control-vb"></a><span data-ttu-id="115f2-104">Hinzufügen von Animationen zu einem Steuerelement (VB)</span><span class="sxs-lookup"><span data-stu-id="115f2-104">Adding Animation to a Control (VB)</span></span>
====================
<span data-ttu-id="115f2-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="115f2-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="115f2-106">[Herunterladen von Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.vb.zip) oder [PDF herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="115f2-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation1.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation1VB.pdf)</span></span>

> <span data-ttu-id="115f2-107">Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="115f2-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="115f2-108">Dieses Lernprogramm zeigt, wie eine solche Animation einrichten.</span><span class="sxs-lookup"><span data-stu-id="115f2-108">This tutorial shows how to set up such an animation.</span></span>


## <a name="overview"></a><span data-ttu-id="115f2-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="115f2-109">Overview</span></span>

<span data-ttu-id="115f2-110">Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="115f2-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="115f2-111">Dieses Lernprogramm zeigt, wie eine solche Animation einrichten.</span><span class="sxs-lookup"><span data-stu-id="115f2-111">This tutorial shows how to set up such an animation.</span></span>

## <a name="steps"></a><span data-ttu-id="115f2-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="115f2-112">Steps</span></span>

<span data-ttu-id="115f2-113">Der erste Schritt besteht wie gewohnt enthalten die `ScriptManager` auf der Seite, damit die ASP.NET AJAX-Bibliothek geladen wird und die Control-Toolkit verwendet werden können:</span><span class="sxs-lookup"><span data-stu-id="115f2-113">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample1.aspx)]

<span data-ttu-id="115f2-114">Die Animation in diesem Szenario wird auf einen Bereich des Texts angewendet werden, die wie folgt aussieht:</span><span class="sxs-lookup"><span data-stu-id="115f2-114">The animation in this scenario will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample2.aspx)]

<span data-ttu-id="115f2-115">Die zugeordnete CSS-Klasse für den Bereich definiert eine Hintergrundfarbe und eine Breitenangabe:</span><span class="sxs-lookup"><span data-stu-id="115f2-115">The associated CSS class for the panel defines a background color and a width:</span></span>

[!code-css[Main](adding-animation-to-a-control-vb/samples/sample3.css)]

<span data-ttu-id="115f2-116">Wir als Nächstes, müssen die `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="115f2-116">Next up, we need the `AnimationExtender`.</span></span> <span data-ttu-id="115f2-117">Nach der Bereitstellung einer `ID` und die üblichen `runat="server"`, die `TargetControlID` Attribut muss an das Steuerelement zu animierende in unserem Fall Bereich festgelegt werden:</span><span class="sxs-lookup"><span data-stu-id="115f2-117">After providing an `ID` and the usual `runat="server"`, the `TargetControlID` attribute must be set to the control to animate in our case, the panel:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample4.aspx)]

<span data-ttu-id="115f2-118">Der gesamte deklarativ Anwendung mithilfe einer XML-Syntax, leider zurzeit nicht vollständig von Visual Studio IntelliSense unterstützt.</span><span class="sxs-lookup"><span data-stu-id="115f2-118">The whole animation is applied declaratively, using an XML syntax, unfortunately currently not fully supported by Visual Studio's IntelliSense.</span></span> <span data-ttu-id="115f2-119">Der Stammknoten entspricht `<Animations>;` innerhalb dieses Knotens sind mehrere Ereignisse zulässig, wenn Animationen Stelle take(s) bestimmen:</span><span class="sxs-lookup"><span data-stu-id="115f2-119">The root node is `<Animations>;` within this node, several events are allowed which determine when the animation(s) take(s) place:</span></span>

- <span data-ttu-id="115f2-120">`OnClick`(Mausklick)</span><span class="sxs-lookup"><span data-stu-id="115f2-120">`OnClick` (mouse click)</span></span>
- <span data-ttu-id="115f2-121">`OnHoverOut`(wenn die Maus für ein Steuerelement verlässt)</span><span class="sxs-lookup"><span data-stu-id="115f2-121">`OnHoverOut` (when the mouse leaves a control)</span></span>
- <span data-ttu-id="115f2-122">`OnHoverOver`(wenn die Maus über ein Steuerelement bewegt wird, beenden die `OnHoverOut` Animation)</span><span class="sxs-lookup"><span data-stu-id="115f2-122">`OnHoverOver` (when the mouse hovers over a control, stopping the `OnHoverOut` animation)</span></span>
- <span data-ttu-id="115f2-123">`OnLoad`(wenn die Seite geladen wurde)</span><span class="sxs-lookup"><span data-stu-id="115f2-123">`OnLoad` (when the page has been loaded)</span></span>
- <span data-ttu-id="115f2-124">`OnMouseOut`(wenn die Maus für ein Steuerelement verlässt)</span><span class="sxs-lookup"><span data-stu-id="115f2-124">`OnMouseOut` (when the mouse leaves a control)</span></span>
- <span data-ttu-id="115f2-125">`OnMouseOver`(wenn die Maus über ein Steuerelement bewegt wird, beenden Sie nicht die `OnMouseOut` Animation)</span><span class="sxs-lookup"><span data-stu-id="115f2-125">`OnMouseOver` (when the mouse hovers over a control, not stopping the `OnMouseOut` animation)</span></span>

<span data-ttu-id="115f2-126">Das Framework enthält eine Reihe von Animationen, jeweils durch eine eigene XML-Element dargestellt wird.</span><span class="sxs-lookup"><span data-stu-id="115f2-126">The framework comes with a set of animations, each one represented by its own XML element.</span></span> <span data-ttu-id="115f2-127">Hier ist eine Auswahl aus:</span><span class="sxs-lookup"><span data-stu-id="115f2-127">Here is a selection:</span></span>

- <span data-ttu-id="115f2-128">`<Color>`(ändern eine Farbe)</span><span class="sxs-lookup"><span data-stu-id="115f2-128">`<Color>` (changing a color)</span></span>
- <span data-ttu-id="115f2-129">`<FadeIn>`(Ausblenden in)</span><span class="sxs-lookup"><span data-stu-id="115f2-129">`<FadeIn>` (fading in)</span></span>
- <span data-ttu-id="115f2-130">`<FadeOut>`(ausblenden)</span><span class="sxs-lookup"><span data-stu-id="115f2-130">`<FadeOut>` (fading out)</span></span>
- <span data-ttu-id="115f2-131">`<Property>`(Ändern eines Steuerelements-Eigenschaft)</span><span class="sxs-lookup"><span data-stu-id="115f2-131">`<Property>` (changing a control's property)</span></span>
- <span data-ttu-id="115f2-132">`<Pulse>`(pulsating)</span><span class="sxs-lookup"><span data-stu-id="115f2-132">`<Pulse>` (pulsating)</span></span>
- <span data-ttu-id="115f2-133">`<Resize>`(Ändern der Größe)</span><span class="sxs-lookup"><span data-stu-id="115f2-133">`<Resize>` (changing the size)</span></span>
- <span data-ttu-id="115f2-134">`<Scale>`(proportional ändern der Größe)</span><span class="sxs-lookup"><span data-stu-id="115f2-134">`<Scale>` (proportionally changing the size)</span></span>

<span data-ttu-id="115f2-135">In diesem Beispiel wird der Bereich ausgeblendet. Die Animation treffen 1,5 Sekunden (`Duration` Attribut), Anzeigen von 24 Frames (Animationsschritte) pro Sekunde (`Fps` Attributs).</span><span class="sxs-lookup"><span data-stu-id="115f2-135">In this example, the panel shall fade out. The animation shall take 1.5 seconds (`Duration` attribute), displaying 24 frames (animation steps) per second (`Fps` attributs).</span></span> <span data-ttu-id="115f2-136">Hier wird das vollständige Markup für die `AnimationExtender` Steuerelement:</span><span class="sxs-lookup"><span data-stu-id="115f2-136">Here is the complete markup for the `AnimationExtender` control:</span></span>

[!code-aspx[Main](adding-animation-to-a-control-vb/samples/sample5.aspx)]

<span data-ttu-id="115f2-137">Wenn Sie dieses Skript ausführen, wird im Bereich angezeigt, und in 1,5 Sekunden ausgeblendet.</span><span class="sxs-lookup"><span data-stu-id="115f2-137">When you run this script, the panel is displayed and fades out in one and a half seconds.</span></span>


<span data-ttu-id="115f2-138">[![Das Panel ausblenden](adding-animation-to-a-control-vb/_static/image2.png)](adding-animation-to-a-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="115f2-138">[![The panel is fading out](adding-animation-to-a-control-vb/_static/image2.png)](adding-animation-to-a-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="115f2-139">Das Panel ausblenden ([klicken Sie hier, um das Bild in voller Größe angezeigt](adding-animation-to-a-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="115f2-139">The panel is fading out ([Click to view full-size image](adding-animation-to-a-control-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="115f2-140">[Zurück](dynamically-controlling-updatepanel-animations-cs.md)
[Weiter](executing-several-animations-at-the-same-time-vb.md)</span><span class="sxs-lookup"><span data-stu-id="115f2-140">[Previous](dynamically-controlling-updatepanel-animations-cs.md)
[Next](executing-several-animations-at-the-same-time-vb.md)</span></span>
