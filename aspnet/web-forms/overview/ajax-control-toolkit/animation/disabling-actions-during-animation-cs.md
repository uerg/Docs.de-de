---
uid: web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
title: Deaktivieren von Aktionen während der Animation (c#) | Microsoft-Dokumentation
author: wenz
description: Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Darüber hinaus wird die Aktion unterstützt...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 918026b4-2f63-421d-8546-df12856960a8
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/disabling-actions-during-animation-cs
msc.type: authoredcontent
ms.openlocfilehash: 4315f06ea1599bacb93a23a3759610e19754cfba
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826057"
---
<a name="disabling-actions-during-animation-c"></a><span data-ttu-id="77533-104">Deaktivieren von Aktionen während der Animation (c#)</span><span class="sxs-lookup"><span data-stu-id="77533-104">Disabling Actions during Animation (C#)</span></span>
====================
<span data-ttu-id="77533-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="77533-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="77533-106">[Code herunterladen](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="77533-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation7.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation7CS.pdf)</span></span>

> <span data-ttu-id="77533-107">Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="77533-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="77533-108">Darüber hinaus werden die Aktionen, wie z.B. Mausklicks unterstützt.</span><span class="sxs-lookup"><span data-stu-id="77533-108">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="77533-109">Beim Klicken mit der Maus eine Animation starten, ist es jedoch wünschenswert, Mausklicks während der Animation zu deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="77533-109">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>


## <a name="overview"></a><span data-ttu-id="77533-110">Übersicht</span><span class="sxs-lookup"><span data-stu-id="77533-110">Overview</span></span>

<span data-ttu-id="77533-111">Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="77533-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="77533-112">Darüber hinaus werden die Aktionen, wie z.B. Mausklicks unterstützt.</span><span class="sxs-lookup"><span data-stu-id="77533-112">It also supports actions, like mouse clicks.</span></span> <span data-ttu-id="77533-113">Beim Klicken mit der Maus eine Animation starten, ist es jedoch wünschenswert, Mausklicks während der Animation zu deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="77533-113">However when a mouse click starts an animation, it is desirable to disable mouse clicks during the animation.</span></span>

## <a name="steps"></a><span data-ttu-id="77533-114">Schritte</span><span class="sxs-lookup"><span data-stu-id="77533-114">Steps</span></span>

<span data-ttu-id="77533-115">Zunächst einmal sind die `ScriptManager` in die Seite klicken Sie dann die ASP.NET AJAX-Bibliothek wird geladen, lässt sich das Steuerelement-Toolkit verwenden:</span><span class="sxs-lookup"><span data-stu-id="77533-115">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample1.aspx)]

<span data-ttu-id="77533-116">Die Animation wird eine HTML-Schaltfläche wie folgt angewendet:</span><span class="sxs-lookup"><span data-stu-id="77533-116">The animation will be applied to an HTML button like this:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample2.aspx)]

<span data-ttu-id="77533-117">Beachten Sie, dass ein HTML-Steuerelement statt eines Websteuerelements verwendet wird, da wir nicht, dass die Schaltfläche zum Erstellen eines Postbacks möchten. Sie müssen nur die clientseitige Animation für uns starten.</span><span class="sxs-lookup"><span data-stu-id="77533-117">Note that an HTML Control is used instead of a Web Control since we do not want the button to create a postback; it shall just launch the client-side animation for us.</span></span>

<span data-ttu-id="77533-118">Fügen Sie dann die `AnimationExtender` auf der Seite Bereitstellen einer `ID`, `TargetControlID` -Attribut und das obligatorische `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="77533-118">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample3.aspx)]

<span data-ttu-id="77533-119">In der `<Animations>` Knoten `<OnClick>` ist das richtige Element Mausklicks zu behandeln.</span><span class="sxs-lookup"><span data-stu-id="77533-119">Within the `<Animations>` node, `<OnClick>` is the right element to handle the mouse click.</span></span> <span data-ttu-id="77533-120">Allerdings konnte die während der Animation auch gedrückt werden.</span><span class="sxs-lookup"><span data-stu-id="77533-120">However, the button could be clicked during the animation, as well.</span></span> <span data-ttu-id="77533-121">Die `<EnableAction>` Element dieser kümmern.</span><span class="sxs-lookup"><span data-stu-id="77533-121">The `<EnableAction>` element can take care of that.</span></span> <span data-ttu-id="77533-122">Festlegen von `Enabled="false"` deaktiviert die Schaltfläche mit den im Rahmen der Animation.</span><span class="sxs-lookup"><span data-stu-id="77533-122">Setting `Enabled="false"` disables the button as part of the animation.</span></span> <span data-ttu-id="77533-123">Da wir mehrere einzelne Animationen (deaktivieren die Schaltfläche und die tatsächliche Animationen), verwenden die `<Parallel>` Element ist erforderlich, um die einzelnen Animationen zusammen in einer kleben.</span><span class="sxs-lookup"><span data-stu-id="77533-123">Since we are using several individual animations (disabling the button and the actual animations), the `<Parallel>` element is required to glue the single animations together into one.</span></span> <span data-ttu-id="77533-124">Hier ist das vollständige Markup für `AnimationExtender`:</span><span class="sxs-lookup"><span data-stu-id="77533-124">Here is the complete markup for `AnimationExtender`:</span></span>

[!code-aspx[Main](disabling-actions-during-animation-cs/samples/sample4.aspx)]

<span data-ttu-id="77533-125">Es wäre auch möglich, auf die Schaltfläche nach der Animation, verwenden das folgende XML-Element am Ende der Liste erneut zu aktivieren:</span><span class="sxs-lookup"><span data-stu-id="77533-125">It would also be possible to re-enable to button after the animation, using the following XML element at the end of the list:</span></span>

[!code-xml[Main](disabling-actions-during-animation-cs/samples/sample5.xml)]

<span data-ttu-id="77533-126">Aber im vorliegenden Szenario dies nutzlos seit der Schaltfläche wäre ausgeblendet wird, und ist nicht sichtbar ist, am Ende der Animation.</span><span class="sxs-lookup"><span data-stu-id="77533-126">However in the given scenario this would be useless since the button fades out and is not visible at the end of the animation.</span></span>


<span data-ttu-id="77533-127">[![Die Schaltfläche ist deaktiviert, sobald die Animation ausgeführt wird](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="77533-127">[![The button is disabled as soon as the animation runs](disabling-actions-during-animation-cs/_static/image2.png)](disabling-actions-during-animation-cs/_static/image1.png)</span></span>

<span data-ttu-id="77533-128">Die Schaltfläche ist deaktiviert, sobald die Animation ausgeführt wird ([klicken Sie, um das Bild in voller Größe anzeigen](disabling-actions-during-animation-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="77533-128">The button is disabled as soon as the animation runs ([Click to view full-size image](disabling-actions-during-animation-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="77533-129">[Zurück](animating-in-response-to-user-interaction-cs.md)
> [Weiter](triggering-an-animation-in-another-control-cs.md)</span><span class="sxs-lookup"><span data-stu-id="77533-129">[Previous](animating-in-response-to-user-interaction-cs.md)
[Next](triggering-an-animation-in-another-control-cs.md)</span></span>
