---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
title: Animieren als Reaktion auf eine Benutzerinteraktion (c#) | Microsoft-Dokumentation
author: wenz
description: Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Die Animationen können Sternen...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: ea26549d-fbbf-4973-a108-b14cd1d6de26
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-in-response-to-user-interaction-cs
msc.type: authoredcontent
ms.openlocfilehash: 5e8ebf5ec7fc0875e0eb43923321513bf0a08899
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37826130"
---
<a name="animating-in-response-to-user-interaction-c"></a><span data-ttu-id="e63d9-104">Animieren als Reaktion auf eine Benutzerinteraktion (c#)</span><span class="sxs-lookup"><span data-stu-id="e63d9-104">Animating in Response To User Interaction (C#)</span></span>
====================
<span data-ttu-id="e63d9-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="e63d9-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="e63d9-106">[Code herunterladen](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.cs.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="e63d9-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation6.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation6CS.pdf)</span></span>

> <span data-ttu-id="e63d9-107">Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="e63d9-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="e63d9-108">Die Animationen können automatisch starten, oder es werden möglicherweise ausgelöst durch einen Benutzereingriff, z. B. indem Sie mit der Maus auf.</span><span class="sxs-lookup"><span data-stu-id="e63d9-108">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>


## <a name="overview"></a><span data-ttu-id="e63d9-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="e63d9-109">Overview</span></span>

<span data-ttu-id="e63d9-110">Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="e63d9-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="e63d9-111">Die Animationen können automatisch starten, oder es werden möglicherweise ausgelöst durch einen Benutzereingriff, z. B. indem Sie mit der Maus auf.</span><span class="sxs-lookup"><span data-stu-id="e63d9-111">The animations can start automatically or may be triggered by user interaction, e.g. by clicking with the mouse.</span></span>

## <a name="steps"></a><span data-ttu-id="e63d9-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="e63d9-112">Steps</span></span>

<span data-ttu-id="e63d9-113">Zunächst einmal sind die `ScriptManager` in die Seite klicken Sie dann die ASP.NET AJAX-Bibliothek wird geladen, lässt sich das Steuerelement-Toolkit verwenden:</span><span class="sxs-lookup"><span data-stu-id="e63d9-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample1.aspx)]

<span data-ttu-id="e63d9-114">Die Animation wird auf einen Bereich des Texts angewendet werden, der so aussieht:</span><span class="sxs-lookup"><span data-stu-id="e63d9-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample2.aspx)]

<span data-ttu-id="e63d9-115">Definieren Sie in der zugehörigen CSS-Klasse für den Bereich eine gute Hintergrundfarbe aus, und auch festlegen Sie eine feste Breite für den Bereich:</span><span class="sxs-lookup"><span data-stu-id="e63d9-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](animating-in-response-to-user-interaction-cs/samples/sample3.css)]

<span data-ttu-id="e63d9-116">Fügen Sie dann die `AnimationExtender` auf der Seite Bereitstellen einer `ID`, `TargetControlID` -Attribut und das obligatorische `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="e63d9-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample4.aspx)]

<span data-ttu-id="e63d9-117">In der `<Animations>` Knoten stehen fünf Methoden zum Starten der Animation über eine Benutzerinteraktion (das fehlende Element ist `<OnLoad>` die wird ausgeführt, nachdem die gesamte Seite vollständig geladen wurde):</span><span class="sxs-lookup"><span data-stu-id="e63d9-117">Within the `<Animations>` node, there are five ways to start the animation via user interaction (the missing element is `<OnLoad>` which is executed once the whole page has been fully loaded):</span></span>

- <span data-ttu-id="e63d9-118">`<OnClick>` (Mausklick auf das Steuerelement)</span><span class="sxs-lookup"><span data-stu-id="e63d9-118">`<OnClick>` (mouse click on the control)</span></span>
- <span data-ttu-id="e63d9-119">`<OnHoverOut>` (Mauszeiger das Steuerelement)</span><span class="sxs-lookup"><span data-stu-id="e63d9-119">`<OnHoverOut>` (mouse leaves the control)</span></span>
- <span data-ttu-id="e63d9-120">`<OnHoverOver>` (Maus bewegt wird, auf ein Steuerelement, das Beenden der `<OnHoverOut>` Animation)</span><span class="sxs-lookup"><span data-stu-id="e63d9-120">`<OnHoverOver>` (mouse hovers over a control, stopping the `<OnHoverOut>` animation)</span></span>
- <span data-ttu-id="e63d9-121">`<OnMouseOut>` (Mauszeiger ein Steuerelement)</span><span class="sxs-lookup"><span data-stu-id="e63d9-121">`<OnMouseOut>` (mouse leaves a control)</span></span>
- <span data-ttu-id="e63d9-122">`<OnMouseOver>` (Maus bewegt wird, auf ein Steuerelement, das nicht beendet die `<OnMouseOut>` Animation)</span><span class="sxs-lookup"><span data-stu-id="e63d9-122">`<OnMouseOver>` (mouse hovers over a control, not stopping the `<OnMouseOut>` animation)</span></span>

<span data-ttu-id="e63d9-123">In diesem Szenario `<OnClick>` verwendet wird.</span><span class="sxs-lookup"><span data-stu-id="e63d9-123">In this scenario, `<OnClick>` is used.</span></span> <span data-ttu-id="e63d9-124">Klickt der Benutzer im Bereich angepasst wird und zur gleichen Zeit ausgeblendet wird.</span><span class="sxs-lookup"><span data-stu-id="e63d9-124">When the user clicks on the panel, it is resized and fades out at the same time.</span></span>

[!code-aspx[Main](animating-in-response-to-user-interaction-cs/samples/sample5.aspx)]


<span data-ttu-id="e63d9-125">[![Startet die Animation, klicken mit der Maus](animating-in-response-to-user-interaction-cs/_static/image2.png)](animating-in-response-to-user-interaction-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e63d9-125">[![A mouse click starts the animation](animating-in-response-to-user-interaction-cs/_static/image2.png)](animating-in-response-to-user-interaction-cs/_static/image1.png)</span></span>

<span data-ttu-id="e63d9-126">Startet die Animation, klicken mit der Maus ([klicken Sie, um das Bild in voller Größe anzeigen](animating-in-response-to-user-interaction-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="e63d9-126">A mouse click starts the animation ([Click to view full-size image](animating-in-response-to-user-interaction-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="e63d9-127">[Zurück](picking-one-animation-out-of-a-list-cs.md)
> [Weiter](disabling-actions-during-animation-cs.md)</span><span class="sxs-lookup"><span data-stu-id="e63d9-127">[Previous](picking-one-animation-out-of-a-list-cs.md)
[Next](disabling-actions-during-animation-cs.md)</span></span>
