---
uid: web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
title: Auswählen einer Animation aus einer Liste (VB) | Microsoft-Dokumentation
author: wenz
description: Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Das Framework auch zulassen...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 81ba9116-d485-40c0-8ff6-7e9ae23e0a0c
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/picking-one-animation-out-of-a-list-vb
msc.type: authoredcontent
ms.openlocfilehash: 9c60d7cff7c841d23185fbdf07abf0e894b21cf5
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827005"
---
<a name="picking-one-animation-out-of-a-list-vb"></a><span data-ttu-id="80202-104">Auswählen einer Animation aus einer Liste (VB)</span><span class="sxs-lookup"><span data-stu-id="80202-104">Picking One Animation Out Of a List (VB)</span></span>
====================
<span data-ttu-id="80202-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="80202-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="80202-106">[Code herunterladen](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.vb.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="80202-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation5.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation5VB.pdf)</span></span>

> <span data-ttu-id="80202-107">Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="80202-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="80202-108">Das Framework ermöglicht es auch den Programmierer, wählen Sie eine Animation aus einer Liste von Animationen, je nach der Auswertung von JavaScript-Code.</span><span class="sxs-lookup"><span data-stu-id="80202-108">The framework also allows the programmer to pick one animation out of a list of animations, depending on the evaluation of some JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="80202-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="80202-109">Overview</span></span>

<span data-ttu-id="80202-110">Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="80202-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="80202-111">Das Framework ermöglicht es auch den Programmierer, wählen Sie eine Animation aus einer Liste von Animationen, je nach der Auswertung von JavaScript-Code.</span><span class="sxs-lookup"><span data-stu-id="80202-111">The framework also allows the programmer to pick one animation out of a list of animations, depending on the evaluation of some JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="80202-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="80202-112">Steps</span></span>

<span data-ttu-id="80202-113">Zunächst einmal sind die `ScriptManager` in die Seite klicken Sie dann die ASP.NET AJAX-Bibliothek wird geladen, lässt sich das Steuerelement-Toolkit verwenden:</span><span class="sxs-lookup"><span data-stu-id="80202-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample1.aspx)]

<span data-ttu-id="80202-114">Die Animation wird auf einen Bereich des Texts angewendet werden, der so aussieht:</span><span class="sxs-lookup"><span data-stu-id="80202-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample2.aspx)]

<span data-ttu-id="80202-115">Definieren Sie in der zugehörigen CSS-Klasse für den Bereich eine gute Hintergrundfarbe aus, und auch festlegen Sie eine feste Breite für den Bereich:</span><span class="sxs-lookup"><span data-stu-id="80202-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](picking-one-animation-out-of-a-list-vb/samples/sample3.css)]

<span data-ttu-id="80202-116">Fügen Sie dann die `AnimationExtender` auf der Seite Bereitstellen einer `ID`, `TargetControlID` -Attribut und das obligatorische `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="80202-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample4.aspx)]

<span data-ttu-id="80202-117">In der `<Animations>` Knoten verwenden `<OnLoad>` die Animationen ausgeführt werden, nachdem die Seite vollständig geladen wurde.</span><span class="sxs-lookup"><span data-stu-id="80202-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="80202-118">Statt in einem regulären-Animationen die `<Case>` Element ins Spiel.</span><span class="sxs-lookup"><span data-stu-id="80202-118">Instead of one of the regular animations, the `<Case>` element comes into play.</span></span> <span data-ttu-id="80202-119">Der Wert seines Attributs SelectScript wird ausgewertet; der Rückgabewert muss numerische sein.</span><span class="sxs-lookup"><span data-stu-id="80202-119">The value of its SelectScript attribute is evaluated; the return value must be numerical.</span></span> <span data-ttu-id="80202-120">Je nach dieser Anzahl, eines der Subanimation in &lt;Fall&gt; ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="80202-120">Depending on this number, one of the subanimations within &lt;Case&gt; is executed.</span></span> <span data-ttu-id="80202-121">Wenn SelectScript 2 ergibt, führt das Steuerelement-Toolkit z. B. dritte Animation in &lt;Fall&gt; (Zählung von 0 beginnt).</span><span class="sxs-lookup"><span data-stu-id="80202-121">For instance, if SelectScript evaluates to 2, the Control Toolkit runs the third animation within &lt;Case&gt; (counting starts at 0).</span></span>

<span data-ttu-id="80202-122">Das folgende Markup definiert drei Subanimation: Ändern Sie die Breite der Größe, ändern Sie die Höhe der Größe und ausgeblendet wird. Der JavaScript-Code (`Math.floor(3 * Math.random())`) wählt anschließend eine Zahl zwischen 0 und 2, sodass einer der drei Animationen ausgeführt wird:</span><span class="sxs-lookup"><span data-stu-id="80202-122">The following markup defines three subanimations: Resizing the width, resizing the height, and fading out. The JavaScript code (`Math.floor(3 * Math.random())`) then picks a number between 0 and 2, so that one of the three animations is run:</span></span>

[!code-aspx[Main](picking-one-animation-out-of-a-list-vb/samples/sample5.aspx)]


<span data-ttu-id="80202-123">[![Einer der drei möglichen Animationen: Ruft der Bereich ab, größere](picking-one-animation-out-of-a-list-vb/_static/image2.png)](picking-one-animation-out-of-a-list-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="80202-123">[![One of the possible three animations: The panel gets wider](picking-one-animation-out-of-a-list-vb/_static/image2.png)](picking-one-animation-out-of-a-list-vb/_static/image1.png)</span></span>

<span data-ttu-id="80202-124">Einer der drei möglichen Animationen: Ruft der Bereich ab, größere ([klicken Sie, um das Bild in voller Größe anzeigen](picking-one-animation-out-of-a-list-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="80202-124">One of the possible three animations: The panel gets wider ([Click to view full-size image](picking-one-animation-out-of-a-list-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="80202-125">[Zurück](animation-depending-on-a-condition-vb.md)
> [Weiter](animating-in-response-to-user-interaction-vb.md)</span><span class="sxs-lookup"><span data-stu-id="80202-125">[Previous](animation-depending-on-a-condition-vb.md)
[Next](animating-in-response-to-user-interaction-vb.md)</span></span>
