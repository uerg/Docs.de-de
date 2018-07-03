---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
title: Ausführen mehrerer Animationen nach anderen (c#) | Microsoft-Dokumentation
author: wenz
description: Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Er ermöglicht es, durch Fallenlassen ausführen...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 7dc02b18-2b5d-4844-b7c5-cbd818477163
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
msc.type: authoredcontent
ms.openlocfilehash: 49d33ee28984981c7757f14fe7c16fb2dc8f744e
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362150"
---
<a name="executing-several-animations-after-each-other-c"></a><span data-ttu-id="7c3c5-104">Ausführen mehrerer Animationen nach anderen (c#)</span><span class="sxs-lookup"><span data-stu-id="7c3c5-104">Executing Several Animations after Each Other (C#)</span></span>
====================
<span data-ttu-id="7c3c5-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="7c3c5-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="7c3c5-106">[Code herunterladen](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.cs.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="7c3c5-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3CS.pdf)</span></span>

> <span data-ttu-id="7c3c5-107">Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="7c3c5-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="7c3c5-108">Sie können zum Ausführen mehrerer Animationen eine nach dem anderen.</span><span class="sxs-lookup"><span data-stu-id="7c3c5-108">It allows to run several animations one after the other.</span></span>


<span data-ttu-id="7c3c5-109">Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="7c3c5-109">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="7c3c5-110">Sie können zum Ausführen mehrerer Animationen eine nach dem anderen.</span><span class="sxs-lookup"><span data-stu-id="7c3c5-110">It allows to run several animations one after the other.</span></span>

## <a name="steps"></a><span data-ttu-id="7c3c5-111">Schritte</span><span class="sxs-lookup"><span data-stu-id="7c3c5-111">Steps</span></span>

<span data-ttu-id="7c3c5-112">Zunächst einmal sind die `ScriptManager` in die Seite klicken Sie dann die ASP.NET AJAX-Bibliothek wird geladen, lässt sich das Steuerelement-Toolkit verwenden:</span><span class="sxs-lookup"><span data-stu-id="7c3c5-112">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample1.aspx)]

<span data-ttu-id="7c3c5-113">Die Animation wird auf einen Bereich des Texts angewendet werden, der so aussieht:</span><span class="sxs-lookup"><span data-stu-id="7c3c5-113">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample2.aspx)]

<span data-ttu-id="7c3c5-114">Definieren Sie in der zugehörigen CSS-Klasse für den Bereich eine gute Hintergrundfarbe aus, und auch festlegen Sie eine feste Breite für den Bereich:</span><span class="sxs-lookup"><span data-stu-id="7c3c5-114">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-several-animations-after-each-other-cs/samples/sample3.css)]

<span data-ttu-id="7c3c5-115">Fügen Sie dann die `AnimationExtender` auf der Seite Bereitstellen einer `ID`, `TargetControlID` -Attribut und das obligatorische `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="7c3c5-115">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample4.aspx)]

<span data-ttu-id="7c3c5-116">In der `<Animations>` Knoten verwenden `<OnLoad>` die Animationen ausgeführt werden, nachdem die Seite vollständig geladen wurde.</span><span class="sxs-lookup"><span data-stu-id="7c3c5-116">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="7c3c5-117">Im allgemeinen `<OnLoad>` akzeptiert nur eine Animation.</span><span class="sxs-lookup"><span data-stu-id="7c3c5-117">Generally, `<OnLoad>` only accepts one animation.</span></span> <span data-ttu-id="7c3c5-118">Der Animation-Framework können Sie mehrere Animationen in einer mit join die `<Sequence>` Element.</span><span class="sxs-lookup"><span data-stu-id="7c3c5-118">The Animation framework allows you to join several animations into one using the `<Sequence>` element.</span></span> <span data-ttu-id="7c3c5-119">Alle Animationen in `<Sequence>` sind, werden nach dem anderen.</span><span class="sxs-lookup"><span data-stu-id="7c3c5-119">All animations within `<Sequence>` are executed one after the other.</span></span> <span data-ttu-id="7c3c5-120">Hier ist die einem möglichen Markup für die `AnimationExtender` Steuerelement, sodass zuerst den größeren Bereich und dann Verringern der Höhe:</span><span class="sxs-lookup"><span data-stu-id="7c3c5-120">Here is the a possible markup for the `AnimationExtender` control, first making the panel wider and then decreasing its height:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample5.aspx)]

<span data-ttu-id="7c3c5-121">Wenn Sie dieses Skript im Bereich erste ruft breiter und dann kleinere ausführen.</span><span class="sxs-lookup"><span data-stu-id="7c3c5-121">When you run this script, the panel first gets wider and then smaller.</span></span>


<span data-ttu-id="7c3c5-122">[![Zuerst wird die Breite erhöht.](executing-several-animations-after-each-other-cs/_static/image2.png)](executing-several-animations-after-each-other-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7c3c5-122">[![First the width is increased](executing-several-animations-after-each-other-cs/_static/image2.png)](executing-several-animations-after-each-other-cs/_static/image1.png)</span></span>

<span data-ttu-id="7c3c5-123">Zuerst wird die Breite erhöht ([klicken Sie, um das Bild in voller Größe anzeigen](executing-several-animations-after-each-other-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="7c3c5-123">First the width is increased ([Click to view full-size image](executing-several-animations-after-each-other-cs/_static/image3.png))</span></span>


<span data-ttu-id="7c3c5-124">[![Anschließend wird die Höhe verringert.](executing-several-animations-after-each-other-cs/_static/image5.png)](executing-several-animations-after-each-other-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="7c3c5-124">[![Then the height is decreased](executing-several-animations-after-each-other-cs/_static/image5.png)](executing-several-animations-after-each-other-cs/_static/image4.png)</span></span>

<span data-ttu-id="7c3c5-125">Und dann die Höhe verringert wird ([klicken Sie, um das Bild in voller Größe anzeigen](executing-several-animations-after-each-other-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="7c3c5-125">Then the height is decreased ([Click to view full-size image](executing-several-animations-after-each-other-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7c3c5-126">[Zurück](executing-several-animations-at-the-same-time-cs.md)
> [Weiter](animation-depending-on-a-condition-cs.md)</span><span class="sxs-lookup"><span data-stu-id="7c3c5-126">[Previous](executing-several-animations-at-the-same-time-cs.md)
[Next](animation-depending-on-a-condition-cs.md)</span></span>
