---
uid: web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-cs
title: Dynamisches Steuern von UpdatePanel-Animationen (c#) | Microsoft-Dokumentation
author: wenz
description: Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Für den Inhalt einer...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 5138b8fe-98ff-4e73-a00b-e263fc3ff11d
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/dynamically-controlling-updatepanel-animations-cs
msc.type: authoredcontent
ms.openlocfilehash: f7b363a23c05490522705648fabfe0e7d0856b55
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400680"
---
<a name="dynamically-controlling-updatepanel-animations-c"></a><span data-ttu-id="eefc0-104">Dynamisches Steuern von UpdatePanel-Animationen (c#)</span><span class="sxs-lookup"><span data-stu-id="eefc0-104">Dynamically Controlling UpdatePanel Animations (C#)</span></span>
====================
<span data-ttu-id="eefc0-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="eefc0-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="eefc0-106">[Code herunterladen](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.cs.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="eefc0-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/UpdatePanelAnimation2.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/updatepanelanimation2CS.pdf)</span></span>

> <span data-ttu-id="eefc0-107">Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="eefc0-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="eefc0-108">Für den Inhalt von einem UpdatePanel-Steuerelement, ein spezieller Extender vorhanden ist, die stark auf die Animationsframework basieren: UpdatePanelAnimation.</span><span class="sxs-lookup"><span data-stu-id="eefc0-108">For the contents of an UpdatePanel, a special extender exists that relies heavily on the animation framework: UpdatePanelAnimation.</span></span> <span data-ttu-id="eefc0-109">Sie können auch zusammen mit UpdatePanel-Triggern arbeiten.</span><span class="sxs-lookup"><span data-stu-id="eefc0-109">It can also work together with UpdatePanel triggers.</span></span>


## <a name="overview"></a><span data-ttu-id="eefc0-110">Übersicht</span><span class="sxs-lookup"><span data-stu-id="eefc0-110">Overview</span></span>

<span data-ttu-id="eefc0-111">Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="eefc0-111">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="eefc0-112">Für den Inhalt einer `UpdatePanel`, ein spezieller Extender vorhanden ist, die sehr auf die Animationsframework basiert: `UpdatePanelAnimation`.</span><span class="sxs-lookup"><span data-stu-id="eefc0-112">For the contents of an `UpdatePanel`, a special extender exists that relies heavily on the animation framework: `UpdatePanelAnimation`.</span></span> <span data-ttu-id="eefc0-113">Sie können auch arbeiten zusammen mit `UpdatePanel` Trigger.</span><span class="sxs-lookup"><span data-stu-id="eefc0-113">It can also work together with `UpdatePanel` triggers.</span></span>

## <a name="steps"></a><span data-ttu-id="eefc0-114">Schritte</span><span class="sxs-lookup"><span data-stu-id="eefc0-114">Steps</span></span>

<span data-ttu-id="eefc0-115">Der erste Schritt ist wie üblich, enthalten die `ScriptManager` auf der Seite, damit die ASP.NET AJAX-Bibliothek geladen wird und das Steuerelement-Toolkit verwendet werden kann:</span><span class="sxs-lookup"><span data-stu-id="eefc0-115">The first step is as usual to include the `ScriptManager` in the page so that the ASP.NET AJAX library is loaded and the Control Toolkit can be used:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample1.aspx)]

<span data-ttu-id="eefc0-116">Die Animation in diesem Szenario wird an eine Anzeige der aktuellen Zeit angewendet werden.</span><span class="sxs-lookup"><span data-stu-id="eefc0-116">The animation in this scenario will be applied to a display of the current time.</span></span> <span data-ttu-id="eefc0-117">Diese Informationen kann geschrieben werden, in eine Bezeichnung mit der `Page_Load()` -Methode, oder (aus Gründen der Einfachheit) den folgenden Inlinecode verwendet wird:</span><span class="sxs-lookup"><span data-stu-id="eefc0-117">This information can be written into a label using the `Page_Load()` method, or (for the sake of simplicity) the following inline code is used:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample2.aspx)]

<span data-ttu-id="eefc0-118">Darüber hinaus wird eine Schaltfläche ausgelöst wird, aktualisieren die Zeit erstellt:</span><span class="sxs-lookup"><span data-stu-id="eefc0-118">Also, a button to trigger updating the time is created:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample3.aspx)]

<span data-ttu-id="eefc0-119">Dieser Code wird dann in eingefügt der `<ContentTemplate>` Teil einer `UpdatePanel` Element.</span><span class="sxs-lookup"><span data-stu-id="eefc0-119">This code is then put into the `<ContentTemplate>` section of an `UpdatePanel` element.</span></span> <span data-ttu-id="eefc0-120">Der Bereich `UpdateMode` Attribut muss festgelegt werden, um `"Conditional"`, da nur Trigger, den Inhalt des Bereichs aktualisieren können.</span><span class="sxs-lookup"><span data-stu-id="eefc0-120">The panel's `UpdateMode` attribute must be set to `"Conditional"`, since only triggers may update the panel's contents.</span></span> <span data-ttu-id="eefc0-121">In der `<Triggers>` Teil der `UpdatePanel`, asynchroner Postbacktrigger erstellt und gebunden werden, um die `Click` -Ereignis der Schaltfläche.</span><span class="sxs-lookup"><span data-stu-id="eefc0-121">In the `<Triggers>` section of the `UpdatePanel`, an asynchronous postback trigger is created and tied to the `Click` event of the button.</span></span> <span data-ttu-id="eefc0-122">Also, wenn der Benutzer auf die Schaltfläche klickt der `UpdatePanel` aktualisiert wird.</span><span class="sxs-lookup"><span data-stu-id="eefc0-122">Thus, if the user clicks on the button, the `UpdatePanel` is refreshed.</span></span> <span data-ttu-id="eefc0-123">Dies ist das Markup für die `UpdatePanel` Steuerelement:</span><span class="sxs-lookup"><span data-stu-id="eefc0-123">Here is the markup for the `UpdatePanel` control:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample4.aspx)]

<span data-ttu-id="eefc0-124">Schließlich die `UpdatePanelAnimationExtender` muss so konfiguriert werden: Legen Sie die `TargetControlID` -Attribut auf die ID des Bereichs, und definieren Sie eine Animation in der Extender.</span><span class="sxs-lookup"><span data-stu-id="eefc0-124">Finally, the `UpdatePanelAnimationExtender` must be configured: Set the `TargetControlID` attribute to the ID of the panel, and define an animation within the extender.</span></span> <span data-ttu-id="eefc0-125">Überblenden in macht Sinn, die einen gute visual Schwerpunkt auf die aktualisierte Zeit erstellt.</span><span class="sxs-lookup"><span data-stu-id="eefc0-125">Fading in makes sense, which creates a nice visual emphasis on the updated time.</span></span> <span data-ttu-id="eefc0-126">Das Extendermarkup kann dann wie folgt aussehen:</span><span class="sxs-lookup"><span data-stu-id="eefc0-126">Your extender markup may then look like this:</span></span>


[!code-aspx[Main](dynamically-controlling-updatepanel-animations-cs/samples/sample5.aspx)]

<span data-ttu-id="eefc0-127">Führen Sie die Datei im Browser.</span><span class="sxs-lookup"><span data-stu-id="eefc0-127">Run the file in the browser.</span></span> <span data-ttu-id="eefc0-128">Wenn Sie auf die Schaltfläche klicken, wird die aktuelle Uhrzeit im Bereich immer für die Dauer von einer Sekunde mit im Verlauf angezeigt.</span><span class="sxs-lookup"><span data-stu-id="eefc0-128">Whenever you click on the button, the current time is shown in the panel, always fading in for the duration of one second.</span></span>


<span data-ttu-id="eefc0-129">[![Die aktuelle Uhrzeit ist mit im Verlauf.](dynamically-controlling-updatepanel-animations-cs/_static/image2.png)](dynamically-controlling-updatepanel-animations-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="eefc0-129">[![The current time is fading in](dynamically-controlling-updatepanel-animations-cs/_static/image2.png)](dynamically-controlling-updatepanel-animations-cs/_static/image1.png)</span></span>

<span data-ttu-id="eefc0-130">Die aktuelle Uhrzeit wird eingeblendet, ([klicken Sie, um das Bild in voller Größe anzeigen](dynamically-controlling-updatepanel-animations-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="eefc0-130">The current time is fading in ([Click to view full-size image](dynamically-controlling-updatepanel-animations-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="eefc0-131">[Zurück](animating-an-updatepanel-control-cs.md)
> [Weiter](adding-animation-to-a-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="eefc0-131">[Previous](animating-an-updatepanel-control-cs.md)
[Next](adding-animation-to-a-control-vb.md)</span></span>
