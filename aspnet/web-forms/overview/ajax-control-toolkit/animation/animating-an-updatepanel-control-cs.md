---
uid: web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-cs
title: Animieren eines UpdatePanel-Steuerelements (c#) | Microsoft Docs
author: wenz
description: Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. Für den Inhalt einer...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e57f8c7c-3940-4bc0-9468-3a0ca69158ea
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/animating-an-updatepanel-control-cs
msc.type: authoredcontent
ms.openlocfilehash: 5d8d5b9c3f15b39045b5e01b455bdddfc9443a24
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="animating-an-updatepanel-control-c"></a><span data-ttu-id="5e78b-104">Animieren eines UpdatePanel-Steuerelements (c#)</span><span class="sxs-lookup"><span data-stu-id="5e78b-104">Animating an UpdatePanel Control (C#)</span></span>
====================
<span data-ttu-id="5e78b-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="5e78b-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="5e78b-106">[Herunterladen von Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.cs.zip) oder [PDF herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="5e78b-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation1CS.pdf)</span></span>

> <span data-ttu-id="5e78b-107">Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="5e78b-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="5e78b-108">Für den Inhalt eines UpdatePanel, ein spezieller Extender vorhanden ist, die sich stark auf die Animationsframework stützt: UpdatePanelAnimation.</span><span class="sxs-lookup"><span data-stu-id="5e78b-108">For the contents of an UpdatePanel, a special extender exists that relies heavily on the animation framework: UpdatePanelAnimation.</span></span> <span data-ttu-id="5e78b-109">In diesem Lernprogramm wird gezeigt, wie solche eine Animation für UpdatePanel eingerichtet wird.</span><span class="sxs-lookup"><span data-stu-id="5e78b-109">This tutorial shows how to set up such an animation for an UpdatePanel.</span></span>


## <a name="overview"></a><span data-ttu-id="5e78b-110">Übersicht</span><span class="sxs-lookup"><span data-stu-id="5e78b-110">Overview</span></span>

<span data-ttu-id="5e78b-111">Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="5e78b-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="5e78b-112">Für den Inhalt einer `UpdatePanel`, ein spezieller Extender vorhanden ist, die sich stark auf die Animationsframework stützt: `UpdatePanelAnimation`.</span><span class="sxs-lookup"><span data-stu-id="5e78b-112">For the contents of an `UpdatePanel`, a special extender exists that relies heavily on the animation framework: `UpdatePanelAnimation`.</span></span> <span data-ttu-id="5e78b-113">In diesem Lernprogramm wird gezeigt, wie solche eine Animation für Einrichten einer `UpdatePanel`.</span><span class="sxs-lookup"><span data-stu-id="5e78b-113">This tutorial shows how to set up such an animation for an `UpdatePanel`.</span></span>

## <a name="steps"></a><span data-ttu-id="5e78b-114">Schritte</span><span class="sxs-lookup"><span data-stu-id="5e78b-114">Steps</span></span>

<span data-ttu-id="5e78b-115">Der erste Schritt besteht wie gewohnt enthalten die `ScriptManager` auf der Seite, damit die ASP.NET AJAX-Bibliothek geladen wird und die Control-Toolkit verwendet werden können:</span><span class="sxs-lookup"><span data-stu-id="5e78b-115">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-cs/samples/sample1.aspx)]

<span data-ttu-id="5e78b-116">Die Animation in diesem Szenario gelten für eine ASP.NET `Wizard` Websteuerelement in den ein `UpdatePanel`.</span><span class="sxs-lookup"><span data-stu-id="5e78b-116">The animation in this scenario will be applied to an ASP.NET `Wizard` web control residing in an `UpdatePanel`.</span></span> <span data-ttu-id="5e78b-117">Drei (beliebigen) Schritte bieten genügend Optionen Postbacks auslösen:</span><span class="sxs-lookup"><span data-stu-id="5e78b-117">Three (arbitrary) steps provide enough options to trigger postbacks:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-cs/samples/sample2.aspx)]

<span data-ttu-id="5e78b-118">Das Markup für die `UpdatePanelAnimationExtender` Steuerelement sind sehr ähnlich, um das Markup, das für die `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="5e78b-118">The markup necessary for the `UpdatePanelAnimationExtender` control is quite similar to the markup used for the `AnimationExtender`.</span></span> <span data-ttu-id="5e78b-119">In der `TargetControlID` Attribut wir bieten die `ID` von der `UpdatePanel` zu animierende; innerhalb der `UpdatePanelAnimationExtender` -Steuerelement, die `<Animations>` -Element enthält XML-Markup für Animationen.</span><span class="sxs-lookup"><span data-stu-id="5e78b-119">In the `TargetControlID` attribute we provide the `ID` of the `UpdatePanel` to animate; within the `UpdatePanelAnimationExtender` control, the `<Animations>` element holds XML markup for the animation(s).</span></span> <span data-ttu-id="5e78b-120">Allerdings gibt es einen Unterschied: die Menge der Ereignisse und Ereignishandler ist im Vergleich zur beschränkt `AnimationExtender`.</span><span class="sxs-lookup"><span data-stu-id="5e78b-120">However there is one difference: The amount of events and event handlers is limited in comparison to `AnimationExtender`.</span></span> <span data-ttu-id="5e78b-121">Für `UpdatePanels`, nur zwei davon vorhanden:</span><span class="sxs-lookup"><span data-stu-id="5e78b-121">For `UpdatePanels`, only two of them exist:</span></span>

- <span data-ttu-id="5e78b-122">`<OnUpdated>` Wenn die UpdatePanel aktualisiert wurde</span><span class="sxs-lookup"><span data-stu-id="5e78b-122">`<OnUpdated>` when the UpdatePanel has been updated</span></span>
- <span data-ttu-id="5e78b-123">`<OnUpdating>` Beim Starten der UpdatePanel aktualisieren</span><span class="sxs-lookup"><span data-stu-id="5e78b-123">`<OnUpdating>` when the UpdatePanel starts updating</span></span>

<span data-ttu-id="5e78b-124">In diesem Szenario wird der neue Inhalt der `UpdatePanel` (nach dem Postback) eingeblendet wird.</span><span class="sxs-lookup"><span data-stu-id="5e78b-124">In this scenario, the new content of the `UpdatePanel` (after the postback) shall fade in.</span></span> <span data-ttu-id="5e78b-125">Dies ist die notwendige Markup für diese:</span><span class="sxs-lookup"><span data-stu-id="5e78b-125">This is the necessary markup for that:</span></span>

[!code-aspx[Main](animating-an-updatepanel-control-cs/samples/sample3.aspx)]

<span data-ttu-id="5e78b-126">Jetzt bei jedem asynchronen in UpdatePanel Postback Einblenden der neue Inhalt des Bereichs reibungslos.</span><span class="sxs-lookup"><span data-stu-id="5e78b-126">Now whenever a postback occurs within the UpdatePanel, the new contents of the panel fade in smoothly.</span></span>


<span data-ttu-id="5e78b-127">[![Der nächste Assistentenschritt wird im ausblenden](animating-an-updatepanel-control-cs/_static/image2.png)](animating-an-updatepanel-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="5e78b-127">[![The next wizard step is fading in](animating-an-updatepanel-control-cs/_static/image2.png)](animating-an-updatepanel-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="5e78b-128">Der nächste Assistentenschritt wird im ausblenden ([klicken Sie hier, um das Bild in voller Größe angezeigt](animating-an-updatepanel-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="5e78b-128">The next wizard step is fading in ([Click to view full-size image](animating-an-updatepanel-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="5e78b-129">[Zurück](changing-an-animation-using-client-side-code-cs.md)
> [Weiter](dynamically-controlling-updatepanel-animations-cs.md)</span><span class="sxs-lookup"><span data-stu-id="5e78b-129">[Previous](changing-an-animation-using-client-side-code-cs.md)
[Next](dynamically-controlling-updatepanel-animations-cs.md)</span></span>
