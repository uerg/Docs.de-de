---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
title: Erstellen sich gegenseitig ausschließende Kontrollkästchen (c#) | Microsoft Docs
author: wenz
description: 'Wenn nur eine der Optionen ausgewählt werden kann, werden in der Regel Optionsfelder verwendet. Obwohl ein Nachteil besteht: Nachdem ein Optionsfeld in einer Gruppe ausgewählt ist,...'
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 8e11b813-ba0d-4c29-b0f8-f65db6dbef1e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
msc.type: authoredcontent
ms.openlocfilehash: c3a5abe7d02ace16f4aaad8d4adfbd0cba8e84ef
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="creating-mutually-exclusive-checkboxes-c"></a><span data-ttu-id="b0ccf-104">Erstellen sich gegenseitig ausschließende Kontrollkästchen (c#)</span><span class="sxs-lookup"><span data-stu-id="b0ccf-104">Creating Mutually Exclusive Checkboxes (C#)</span></span>
====================
<span data-ttu-id="b0ccf-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="b0ccf-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="b0ccf-106">[Herunterladen von Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.cs.zip) oder [PDF herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="b0ccf-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0CS.pdf)</span></span>

> <span data-ttu-id="b0ccf-107">Wenn nur eine der Optionen ausgewählt werden kann, werden in der Regel Optionsfelder verwendet.</span><span class="sxs-lookup"><span data-stu-id="b0ccf-107">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="b0ccf-108">Obwohl ein Nachteil besteht: Nachdem ein Optionsfeld in einer Gruppe ausgewählt ist, ist es nicht möglich, alle Optionsfelder deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="b0ccf-108">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="b0ccf-109">Kontrollkästchen kann zu einem beliebigen Zeitpunkt aufgehoben werden, sind jedoch nicht gegenseitig.</span><span class="sxs-lookup"><span data-stu-id="b0ccf-109">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="b0ccf-110">Dieses Lernprogramm bietet das Beste beider Ansätze: Kontrollkästchen, die sich gegenseitig ausschließende sind.</span><span class="sxs-lookup"><span data-stu-id="b0ccf-110">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>


## <a name="overview"></a><span data-ttu-id="b0ccf-111">Übersicht</span><span class="sxs-lookup"><span data-stu-id="b0ccf-111">Overview</span></span>

<span data-ttu-id="b0ccf-112">Wenn nur eine der Optionen ausgewählt werden kann, werden in der Regel Optionsfelder verwendet.</span><span class="sxs-lookup"><span data-stu-id="b0ccf-112">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="b0ccf-113">Obwohl ein Nachteil besteht: Nachdem ein Optionsfeld in einer Gruppe ausgewählt ist, ist es nicht möglich, alle Optionsfelder deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="b0ccf-113">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="b0ccf-114">Kontrollkästchen kann zu einem beliebigen Zeitpunkt aufgehoben werden, sind jedoch nicht gegenseitig.</span><span class="sxs-lookup"><span data-stu-id="b0ccf-114">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="b0ccf-115">Dieses Lernprogramm bietet das Beste beider Ansätze: Kontrollkästchen, die sich gegenseitig ausschließende sind.</span><span class="sxs-lookup"><span data-stu-id="b0ccf-115">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>

## <a name="steps"></a><span data-ttu-id="b0ccf-116">Schritte</span><span class="sxs-lookup"><span data-stu-id="b0ccf-116">Steps</span></span>

<span data-ttu-id="b0ccf-117">Das ASP.NET AJAX-Steuerelement-Toolkit enthält die MutuallyExclusiveCheckBox Extender.</span><span class="sxs-lookup"><span data-stu-id="b0ccf-117">The ASP.NET AJAX Control Toolkit contains the MutuallyExclusiveCheckBox extender.</span></span> <span data-ttu-id="b0ccf-118">Weisen Sie alle Kontrollkästchen auf einen Gruppennamen Programmierer können sich (`Key` Attribut).</span><span class="sxs-lookup"><span data-stu-id="b0ccf-118">This enables programmers to assign any checkbox to a group name (`Key` attribute).</span></span> <span data-ttu-id="b0ccf-119">Alle Kontrollkästchen innerhalb der gleichen Gruppe darf nur eine gleichzeitig ausgewählt werden.</span><span class="sxs-lookup"><span data-stu-id="b0ccf-119">From all check boxes within the same group, only one may be selected at one time.</span></span>

<span data-ttu-id="b0ccf-120">Beginnen wir mit zwei Kontrollkästchen auf eine neue ASP.NET-Seite ablegen.</span><span class="sxs-lookup"><span data-stu-id="b0ccf-120">Let's start with putting two check boxes on a new ASP.NET page.</span></span> <span data-ttu-id="b0ccf-121">Es können weitere, aber zwei davon reicht aus, um das Prinzip veranschaulichen:</span><span class="sxs-lookup"><span data-stu-id="b0ccf-121">There can be more, but two of them suffice to demonstrate the principle:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample1.aspx)]

<span data-ttu-id="b0ccf-122">Für beide Kontrollkästchen muss ein MutuallyExclusiveCheckBoxExtender-Steuerelement auf der Seite abgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="b0ccf-122">For both checkboxes, a MutuallyExclusiveCheckBoxExtender control must be put on the page.</span></span> <span data-ttu-id="b0ccf-123">Beide Schlüsselattribute müssen den gleichen Wert wie der Wert sein die Attribute des HTML-Radio Button-Elemente mit der Gruppe "bezeichnen, in dem sie angehören identisch sein müssen.</span><span class="sxs-lookup"><span data-stu-id="b0ccf-123">Both Key attributes need to have the same value, just as the value attributes of HTML radio button elements must be identical to denote the group they belong to.</span></span> <span data-ttu-id="b0ccf-124">Die TargetControlID-Eigenschaft des Extenders verweist auf die ID des Kontrollkästchens auf.</span><span class="sxs-lookup"><span data-stu-id="b0ccf-124">The TargetControlID property of the extender points to the ID of the check box.</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample2.aspx)]

<span data-ttu-id="b0ccf-125">Darüber hinaus enthalten die ASP.NET AJAX `ScriptManager` das durch alle Elemente eines ASP.NET AJAX-Steuerelement-Toolkit erforderlich ist:</span><span class="sxs-lookup"><span data-stu-id="b0ccf-125">Finally, include the ASP.NET AJAX `ScriptManager` which is required by all elements of the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample3.aspx)]

<span data-ttu-id="b0ccf-126">Speichern und führen Sie die Seite: Sie aktivieren bzw. deaktivieren Sie beide Kontrollkästchen können jedoch zu keinem Zeitpunkt können beide Kontrollkästchen aktiviert werden.</span><span class="sxs-lookup"><span data-stu-id="b0ccf-126">Save and run the page: You can check and uncheck both check boxes, however at no time can both check boxes be checked.</span></span>


<span data-ttu-id="b0ccf-127">[![Nur ein Kontrollkästchen kann zu einem Zeitpunkt überprüft werden](creating-mutually-exclusive-checkboxes-cs/_static/image2.png)](creating-mutually-exclusive-checkboxes-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b0ccf-127">[![Only one checkbox can be checked at a time](creating-mutually-exclusive-checkboxes-cs/_static/image2.png)](creating-mutually-exclusive-checkboxes-cs/_static/image1.png)</span></span>

<span data-ttu-id="b0ccf-128">Zu einem Zeitpunkt kann nur ein Kontrollkästchen aktiviert werden ([klicken Sie hier, um das Bild in voller Größe angezeigt](creating-mutually-exclusive-checkboxes-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="b0ccf-128">Only one checkbox can be checked at a time ([Click to view full-size image](creating-mutually-exclusive-checkboxes-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="b0ccf-129">Nächste</span><span class="sxs-lookup"><span data-stu-id="b0ccf-129">Next</span></span>](creating-mutually-exclusive-checkboxes-vb.md)
