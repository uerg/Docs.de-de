---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
title: Ausführen mehrerer Animationen nacheinander (VB) | Microsoft-Dokumentation
author: wenz
description: Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Er ermöglicht es, durch Fallenlassen ausführen...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 21ece509-79cc-4d9d-892d-7b6e9c4d3502
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-vb
msc.type: authoredcontent
ms.openlocfilehash: 3d94619ca40d288f8a5a391ef02103902bd2550c
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37835954"
---
<a name="executing-several-animations-after-each-other-vb"></a><span data-ttu-id="4f039-104">Ausführen mehrerer Animationen nacheinander (VB)</span><span class="sxs-lookup"><span data-stu-id="4f039-104">Executing Several Animations after Each Other (VB)</span></span>
====================
<span data-ttu-id="4f039-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="4f039-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="4f039-106">[Code herunterladen](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.vb.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="4f039-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3VB.pdf)</span></span>

> <span data-ttu-id="4f039-107">Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="4f039-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="4f039-108">Sie können zum Ausführen mehrerer Animationen eine nach dem anderen.</span><span class="sxs-lookup"><span data-stu-id="4f039-108">It allows to run several animations one after the other.</span></span>


## <a name="overview"></a><span data-ttu-id="4f039-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="4f039-109">Overview</span></span>

<span data-ttu-id="4f039-110">Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="4f039-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="4f039-111">Sie können zum Ausführen mehrerer Animationen eine nach dem anderen.</span><span class="sxs-lookup"><span data-stu-id="4f039-111">It allows to run several animations one after the other.</span></span>

## <a name="steps"></a><span data-ttu-id="4f039-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="4f039-112">Steps</span></span>

<span data-ttu-id="4f039-113">Zunächst einmal sind die `ScriptManager` in die Seite klicken Sie dann die ASP.NET AJAX-Bibliothek wird geladen, lässt sich das Steuerelement-Toolkit verwenden:</span><span class="sxs-lookup"><span data-stu-id="4f039-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample1.aspx)]

<span data-ttu-id="4f039-114">Die Animation wird auf einen Bereich des Texts angewendet werden, der so aussieht:</span><span class="sxs-lookup"><span data-stu-id="4f039-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample2.aspx)]

<span data-ttu-id="4f039-115">Definieren Sie in der zugehörigen CSS-Klasse für den Bereich eine gute Hintergrundfarbe aus, und auch festlegen Sie eine feste Breite für den Bereich:</span><span class="sxs-lookup"><span data-stu-id="4f039-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-several-animations-after-each-other-vb/samples/sample3.css)]

<span data-ttu-id="4f039-116">Fügen Sie dann die `AnimationExtender` auf der Seite Bereitstellen einer `ID`, `TargetControlID` -Attribut und das obligatorische `runat="server":`</span><span class="sxs-lookup"><span data-stu-id="4f039-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server":`</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample4.aspx)]

<span data-ttu-id="4f039-117">In der `<Animations>` Knoten verwenden `<OnLoad>` die Animationen ausgeführt werden, nachdem die Seite vollständig geladen wurde.</span><span class="sxs-lookup"><span data-stu-id="4f039-117">Within the `<Animations>` node, use `<OnLoad>` to run the animations once the page has been fully loaded.</span></span> <span data-ttu-id="4f039-118">Im allgemeinen `<OnLoad>` akzeptiert nur eine Animation.</span><span class="sxs-lookup"><span data-stu-id="4f039-118">Generally, `<OnLoad>` only accepts one animation.</span></span> <span data-ttu-id="4f039-119">Der Animation-Framework können Sie mehrere Animationen in einer mit join die `<Sequence>` Element.</span><span class="sxs-lookup"><span data-stu-id="4f039-119">The Animation framework allows you to join several animations into one using the `<Sequence>` element.</span></span> <span data-ttu-id="4f039-120">Alle Animationen in `<Sequence>` sind, werden nach dem anderen.</span><span class="sxs-lookup"><span data-stu-id="4f039-120">All animations within `<Sequence>` are executed one after the other.</span></span> <span data-ttu-id="4f039-121">Hier ist die einem möglichen Markup für die `AnimationExtender` Steuerelement, sodass zuerst den größeren Bereich und dann Verringern der Höhe:</span><span class="sxs-lookup"><span data-stu-id="4f039-121">Here is the a possible markup for the `AnimationExtender` control, first making the panel wider and then decreasing its height:</span></span>

[!code-aspx[Main](executing-several-animations-after-each-other-vb/samples/sample5.aspx)]

<span data-ttu-id="4f039-122">Wenn Sie dieses Skript im Bereich erste ruft breiter und dann kleinere ausführen.</span><span class="sxs-lookup"><span data-stu-id="4f039-122">When you run this script, the panel first gets wider and then smaller.</span></span>


<span data-ttu-id="4f039-123">[![Zuerst wird die Breite erhöht.](executing-several-animations-after-each-other-vb/_static/image2.png)](executing-several-animations-after-each-other-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="4f039-123">[![First the width is increased](executing-several-animations-after-each-other-vb/_static/image2.png)](executing-several-animations-after-each-other-vb/_static/image1.png)</span></span>

<span data-ttu-id="4f039-124">Zuerst wird die Breite erhöht ([klicken Sie, um das Bild in voller Größe anzeigen](executing-several-animations-after-each-other-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="4f039-124">First the width is increased ([Click to view full-size image](executing-several-animations-after-each-other-vb/_static/image3.png))</span></span>


<span data-ttu-id="4f039-125">[![Anschließend wird die Höhe verringert.](executing-several-animations-after-each-other-vb/_static/image5.png)](executing-several-animations-after-each-other-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="4f039-125">[![Then the height is decreased](executing-several-animations-after-each-other-vb/_static/image5.png)](executing-several-animations-after-each-other-vb/_static/image4.png)</span></span>

<span data-ttu-id="4f039-126">Und dann die Höhe verringert wird ([klicken Sie, um das Bild in voller Größe anzeigen](executing-several-animations-after-each-other-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="4f039-126">Then the height is decreased ([Click to view full-size image](executing-several-animations-after-each-other-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="4f039-127">[Zurück](executing-several-animations-at-the-same-time-vb.md)
> [Weiter](animation-depending-on-a-condition-vb.md)</span><span class="sxs-lookup"><span data-stu-id="4f039-127">[Previous](executing-several-animations-at-the-same-time-vb.md)
[Next](animation-depending-on-a-condition-vb.md)</span></span>
