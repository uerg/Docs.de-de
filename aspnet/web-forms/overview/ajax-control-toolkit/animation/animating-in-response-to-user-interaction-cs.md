---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
title: Animieren in Reaktion auf Benutzerinteraktionen (c#) | Microsoft Docs
author: wenz
description: "Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. Die Animationen können Stern..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ea26549d-fbbf-4973-a108-b14cd1d6de26
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
msc.type: authoredcontent
ms.openlocfilehash: efb9c34c317ec56b43c498f40a857a9b47fa50b2
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="animating-in-response-to-user-interaction-c"></a><span data-ttu-id="8cd43-104">Animieren in Reaktion auf Benutzerinteraktionen (c#)</span><span class="sxs-lookup"><span data-stu-id="8cd43-104">Animating in Response To User Interaction (C#)</span></span>
====================
<span data-ttu-id="8cd43-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="8cd43-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="8cd43-106">[Herunterladen von Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.cs.zip) oder [PDF herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="8cd43-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6CS.pdf)</span></span>

> <span data-ttu-id="8cd43-107">Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="8cd43-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="8cd43-108">Die Animationen können automatisch starten, oder es können ausgelöst werden durch eine Benutzerinteraktion, z. B. indem Sie mit der Maus auf.</span><span class="sxs-lookup"><span data-stu-id="8cd43-108">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>


## <a name="overview"></a><span data-ttu-id="8cd43-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="8cd43-109">Overview</span></span>

<span data-ttu-id="8cd43-110">Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="8cd43-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="8cd43-111">Die Animationen können automatisch starten, oder es können ausgelöst werden durch eine Benutzerinteraktion, z. B. indem Sie mit der Maus auf.</span><span class="sxs-lookup"><span data-stu-id="8cd43-111">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>

## <a name="steps"></a><span data-ttu-id="8cd43-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="8cd43-112">Steps</span></span>

<span data-ttu-id="8cd43-113">Erstens sind die `ScriptManager` auf der Seite; klicken Sie dann die ASP.NET AJAX-Bibliothek geladen ist, wodurch das Steuerelement-Toolkit verwenden:</span><span class="sxs-lookup"><span data-stu-id="8cd43-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample1.aspx)]

<span data-ttu-id="8cd43-114">Die Animation wird auf einen Bereich des Texts angewendet werden, die wie folgt aussieht:</span><span class="sxs-lookup"><span data-stu-id="8cd43-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample2.aspx)]

<span data-ttu-id="8cd43-115">Definieren Sie in der zugehörigen CSS-Klasse für den Bereich eine gute Hintergrundfarbe und auch festlegen Sie eine feste Breite für den Bereich:</span><span class="sxs-lookup"><span data-stu-id="8cd43-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animating-in-response-to-user-interaction-cs/samples/sample3.css)]

<span data-ttu-id="8cd43-116">Fügen Sie dann die `AnimationExtender` auf der Seite "Bereitstellen einer `ID`, die `TargetControlID` Attribut und der Auswahlparameter `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="8cd43-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample4.aspx)]

<span data-ttu-id="8cd43-117">Innerhalb der `<Animations>` Knoten, es gibt fünf Möglichkeiten zum Starten der Animation über die Interaktion des Benutzers (ist das fehlende Element `<OnLoad>` das ausgeführt wird, nachdem die ganze Seite vollständig geladen wurde):</span><span class="sxs-lookup"><span data-stu-id="8cd43-117">Within the `<Animations>` node, there are five ways to start the animation via user interaction (the missing element is `<OnLoad>` which is executed once the whole page has been fully loaded):</span></span>

- <span data-ttu-id="8cd43-118">`<OnClick>`(Mausklick auf das Steuerelement)</span><span class="sxs-lookup"><span data-stu-id="8cd43-118">`<OnClick>` (mouse click on the control)</span></span>
- <span data-ttu-id="8cd43-119">`<OnHoverOut>`(verlassen hat das Steuerelement der Maus)</span><span class="sxs-lookup"><span data-stu-id="8cd43-119">`<OnHoverOut>` (mouse leaves the control)</span></span>
- <span data-ttu-id="8cd43-120">`<OnHoverOver>`(Maus bewegt wird, über ein Steuerelement, das Beenden der `<OnHoverOut>` Animation)</span><span class="sxs-lookup"><span data-stu-id="8cd43-120">`<OnHoverOver>` (mouse hovers over a control, stopping the `<OnHoverOut>` animation)</span></span>
- <span data-ttu-id="8cd43-121">`<OnMouseOut>`(ein Steuerelement verlässt Maus)</span><span class="sxs-lookup"><span data-stu-id="8cd43-121">`<OnMouseOut>` (mouse leaves a control)</span></span>
- <span data-ttu-id="8cd43-122">`<OnMouseOver>`(Maus bewegt wird, über ein Steuerelement, Beenden nicht die `<OnMouseOut>` Animation)</span><span class="sxs-lookup"><span data-stu-id="8cd43-122">`<OnMouseOver>` (mouse hovers over a control, not stopping the `<OnMouseOut>` animation)</span></span>

<span data-ttu-id="8cd43-123">In diesem Szenario `<OnClick>` verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="8cd43-123">In this scenario, `<OnClick>` is used.</span></span> <span data-ttu-id="8cd43-124">Klickt der Benutzer im Bereich wird skaliert, und gleichzeitig ausgeblendet.</span><span class="sxs-lookup"><span data-stu-id="8cd43-124">When the user clicks on the panel, it is resized and fades out at the same time.</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample5.aspx)]


<span data-ttu-id="8cd43-125">[![Startet die Animation, klicken mit der Maus](animating-in-response-to-user-interaction-cs/_static/image2.png)](animating-in-response-to-user-interaction-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8cd43-125">[![A mouse click starts the animation](animating-in-response-to-user-interaction-cs/_static/image2.png)](animating-in-response-to-user-interaction-cs/_static/image1.png)</span></span>

<span data-ttu-id="8cd43-126">Startet die Animation, klicken mit der Maus ([klicken Sie hier, um das Bild in voller Größe angezeigt](animating-in-response-to-user-interaction-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="8cd43-126">A mouse click starts the animation ([Click to view full-size image](animating-in-response-to-user-interaction-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="8cd43-127">[Zurück](picking-one-animation-out-of-a-list-cs.md)
[Weiter](disabling-actions-during-animation-cs.md)</span><span class="sxs-lookup"><span data-stu-id="8cd43-127">[Previous](picking-one-animation-out-of-a-list-cs.md)
[Next](disabling-actions-during-animation-cs.md)</span></span>
