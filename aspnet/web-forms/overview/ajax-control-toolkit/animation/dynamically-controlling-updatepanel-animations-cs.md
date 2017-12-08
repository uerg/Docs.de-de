---
uid: web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-cs
title: UpdatePanel-Animationen (c#) dynamisch steuern | Microsoft Docs
author: wenz
description: "Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. Für den Inhalt einer..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 5138b8fe-98ff-4e73-a00b-e263fc3ff11d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-cs
msc.type: authoredcontent
ms.openlocfilehash: 0408556e322a46211388fbd557d7a967044129ef
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="dynamically-controlling-updatepanel-animations-c"></a><span data-ttu-id="75300-104">UpdatePanel-Animationen dynamisch steuern (c#)</span><span class="sxs-lookup"><span data-stu-id="75300-104">Dynamically Controlling UpdatePanel Animations (C#)</span></span>
====================
<span data-ttu-id="75300-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="75300-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="75300-106">[Herunterladen von Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.cs.zip) oder [PDF herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="75300-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2CS.pdf)</span></span>

> <span data-ttu-id="75300-107">Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="75300-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="75300-108">Für den Inhalt eines UpdatePanel, ein spezieller Extender vorhanden ist, die sich stark auf die Animationsframework stützt: UpdatePanelAnimation.</span><span class="sxs-lookup"><span data-stu-id="75300-108">For the contents of an UpdatePanel, a special extender exists that relies heavily on the animation framework: UpdatePanelAnimation.</span></span> <span data-ttu-id="75300-109">Sie können auch zusammen mit UpdatePanel Trigger arbeiten.</span><span class="sxs-lookup"><span data-stu-id="75300-109">It can also work together with UpdatePanel triggers.</span></span>


## <a name="overview"></a><span data-ttu-id="75300-110">Übersicht</span><span class="sxs-lookup"><span data-stu-id="75300-110">Overview</span></span>

<span data-ttu-id="75300-111">Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="75300-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="75300-112">Für den Inhalt einer `UpdatePanel`, ein spezieller Extender vorhanden ist, die sich stark auf die Animationsframework stützt: `UpdatePanelAnimation`.</span><span class="sxs-lookup"><span data-stu-id="75300-112">For the contents of an `UpdatePanel`, a special extender exists that relies heavily on the animation framework: `UpdatePanelAnimation`.</span></span> <span data-ttu-id="75300-113">Es kann auch zusammen mit arbeiten `UpdatePanel` Trigger.</span><span class="sxs-lookup"><span data-stu-id="75300-113">It can also work together with `UpdatePanel` triggers.</span></span>

## <a name="steps"></a><span data-ttu-id="75300-114">Schritte</span><span class="sxs-lookup"><span data-stu-id="75300-114">Steps</span></span>

<span data-ttu-id="75300-115">Der erste Schritt besteht wie gewohnt enthalten die `ScriptManager` auf der Seite, damit die ASP.NET AJAX-Bibliothek geladen wird und die Control-Toolkit verwendet werden können:</span><span class="sxs-lookup"><span data-stu-id="75300-115">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample1.aspx)]

<span data-ttu-id="75300-116">Die Animation in diesem Szenario wird an eine Anzeige der aktuellen Uhrzeit angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="75300-116">The animation in this scenario will be applied to a display of the current time.</span></span> <span data-ttu-id="75300-117">Diese Informationen kann geschrieben werden, in eine Bezeichnung mit der `Page_Load()` -Methode, oder (der Einfachheit halber) die folgenden Inlinecode verwendet wird:</span><span class="sxs-lookup"><span data-stu-id="75300-117">This information can be written into a label using the `Page_Load()` method, or (for the sake of simplicity) the following inline code is used:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample2.aspx)]

<span data-ttu-id="75300-118">Darüber hinaus wird eine Schaltfläche zum Auslösen der Zeit aktualisiert erstellt:</span><span class="sxs-lookup"><span data-stu-id="75300-118">Also, a button to trigger updating the time is created:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample3.aspx)]

<span data-ttu-id="75300-119">Dieser Code wird dann in eingefügt die `<ContentTemplate>` Teil einer `UpdatePanel` Element.</span><span class="sxs-lookup"><span data-stu-id="75300-119">This code is then put into the `<ContentTemplate>` section of an `UpdatePanel` element.</span></span> <span data-ttu-id="75300-120">Im Bereich `UpdateMode` Attribut muss festgelegt werden, um `"Conditional"`, da nur Trigger möglicherweise den Inhalt des Bereichs aktualisiert.</span><span class="sxs-lookup"><span data-stu-id="75300-120">The panel's `UpdateMode` attribute must be set to `"Conditional"`, since only triggers may update the panel's contents.</span></span> <span data-ttu-id="75300-121">In der `<Triggers>` Teil der `UpdatePanel`, asynchroner Postbacks Trigger erstellt und gebunden werden, um die `Click` -Ereignis der Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="75300-121">In the `<Triggers>` section of the `UpdatePanel`, an asynchronous postback trigger is created and tied to the `Click` event of the button.</span></span> <span data-ttu-id="75300-122">Daher, wenn der Benutzer auf die Schaltfläche klickt der `UpdatePanel` aktualisiert wird.</span><span class="sxs-lookup"><span data-stu-id="75300-122">Thus, if the user clicks on the button, the `UpdatePanel` is refreshed.</span></span> <span data-ttu-id="75300-123">Hier wird das Markup für die `UpdatePanel` Steuerelement:</span><span class="sxs-lookup"><span data-stu-id="75300-123">Here is the markup for the `UpdatePanel` control:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample4.aspx)]

<span data-ttu-id="75300-124">Schließlich die `UpdatePanelAnimationExtender` muss so konfiguriert werden: Legen Sie die `TargetControlID` -Attribut auf die ID des Bereichs, und definieren Sie eine Animation in der Extender.</span><span class="sxs-lookup"><span data-stu-id="75300-124">Finally, the `UpdatePanelAnimationExtender` must be configured: Set the `TargetControlID` attribute to the ID of the panel, and define an animation within the extender.</span></span> <span data-ttu-id="75300-125">Ausblenden im ist sinnvoll, die einen gute visual Schwerpunkt auf die aktualisierte Uhrzeit erstellt.</span><span class="sxs-lookup"><span data-stu-id="75300-125">Fading in makes sense, which creates a nice visual emphasis on the updated time.</span></span> <span data-ttu-id="75300-126">Das Extendermarkup kann dann wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="75300-126">Your extender markup may then look like this:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample5.aspx)]

<span data-ttu-id="75300-127">Führen Sie die Datei im Browser.</span><span class="sxs-lookup"><span data-stu-id="75300-127">Run the file in the browser.</span></span> <span data-ttu-id="75300-128">Wenn Sie auf die Schaltfläche klicken, wird die aktuelle Uhrzeit im Bereich für die Dauer von einer Sekunde immer einblenden angezeigt.</span><span class="sxs-lookup"><span data-stu-id="75300-128">Whenever you click on the button, the current time is shown in the panel, always fading in for the duration of one second.</span></span>


<span data-ttu-id="75300-129">[![Die aktuelle Uhrzeit ist einblenden](dynamically-controlling-updatepanel-animations-cs/_static/image2.png)](dynamically-controlling-updatepanel-animations-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="75300-129">[![The current time is fading in](dynamically-controlling-updatepanel-animations-cs/_static/image2.png)](dynamically-controlling-updatepanel-animations-cs/_static/image1.png)</span></span>

<span data-ttu-id="75300-130">Die aktuelle Uhrzeit ist einblenden ([klicken Sie hier, um das Bild in voller Größe angezeigt](dynamically-controlling-updatepanel-animations-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="75300-130">The current time is fading in ([Click to view full-size image](dynamically-controlling-updatepanel-animations-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="75300-131">[Zurück](animating-an-updatepanel-control-cs.md)
[Weiter](adding-animation-to-a-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="75300-131">[Previous](animating-an-updatepanel-control-cs.md)
[Next](adding-animation-to-a-control-vb.md)</span></span>
