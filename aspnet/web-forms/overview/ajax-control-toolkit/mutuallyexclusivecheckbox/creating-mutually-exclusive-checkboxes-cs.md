---
uid: web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
title: Erstellen von sich gegenseitig ausschließenden Kontrollkästchen (c#) | Microsoft-Dokumentation
author: wenz
description: 'Wenn nur eine der Optionen ausgewählt werden kann, werden die Optionsfelder in der Regel verwendet. Besteht Sie jedoch ein Nachteil: Wenn in einer Gruppe ein Optionsfeld ausgewählt ist,...'
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 8e11b813-ba0d-4c29-b0f8-f65db6dbef1e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/mutuallyexclusivecheckbox/creating-mutually-exclusive-checkboxes-cs
msc.type: authoredcontent
ms.openlocfilehash: 1bce8cd94ed11551f75e48b19cd9cc50b9d023c9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37818062"
---
<a name="creating-mutually-exclusive-checkboxes-c"></a><span data-ttu-id="8f18b-104">Erstellen von sich gegenseitig ausschließenden Kontrollkästchen (c#)</span><span class="sxs-lookup"><span data-stu-id="8f18b-104">Creating Mutually Exclusive Checkboxes (C#)</span></span>
====================
<span data-ttu-id="8f18b-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="8f18b-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="8f18b-106">[Code herunterladen](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.cs.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="8f18b-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/MutuallyExclusiveCheckBox0.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/mutuallyexclusivecheckbox0CS.pdf)</span></span>

> <span data-ttu-id="8f18b-107">Wenn nur eine der Optionen ausgewählt werden kann, werden die Optionsfelder in der Regel verwendet.</span><span class="sxs-lookup"><span data-stu-id="8f18b-107">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="8f18b-108">Ist es ein Nachteil ist, jedoch: Sobald in einer Gruppe ein Optionsfeld ausgewählt ist, es ist nicht möglich, alle Optionsfelder zu deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="8f18b-108">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="8f18b-109">Kontrollkästchen kann jederzeit deaktiviert werden, jedoch sind nicht gegenseitig.</span><span class="sxs-lookup"><span data-stu-id="8f18b-109">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="8f18b-110">In diesem Tutorial werden die Vorteile beider Ansätze: Kontrollkästchen, die sich gegenseitig.</span><span class="sxs-lookup"><span data-stu-id="8f18b-110">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>


## <a name="overview"></a><span data-ttu-id="8f18b-111">Übersicht</span><span class="sxs-lookup"><span data-stu-id="8f18b-111">Overview</span></span>

<span data-ttu-id="8f18b-112">Wenn nur eine der Optionen ausgewählt werden kann, werden die Optionsfelder in der Regel verwendet.</span><span class="sxs-lookup"><span data-stu-id="8f18b-112">When only one of a set of options may be selected, radio buttons are usually used.</span></span> <span data-ttu-id="8f18b-113">Ist es ein Nachteil ist, jedoch: Sobald in einer Gruppe ein Optionsfeld ausgewählt ist, es ist nicht möglich, alle Optionsfelder zu deaktivieren.</span><span class="sxs-lookup"><span data-stu-id="8f18b-113">There is a drawback, though: Once one radio button in a group is selected, it is not possible to uncheck all radio buttons.</span></span> <span data-ttu-id="8f18b-114">Kontrollkästchen kann jederzeit deaktiviert werden, jedoch sind nicht gegenseitig.</span><span class="sxs-lookup"><span data-stu-id="8f18b-114">Check boxes can be unchecked at any time, however are not mutually exclusive.</span></span> <span data-ttu-id="8f18b-115">In diesem Tutorial werden die Vorteile beider Ansätze: Kontrollkästchen, die sich gegenseitig.</span><span class="sxs-lookup"><span data-stu-id="8f18b-115">This tutorial provides the best of both approaches: check boxes that are mutually exclusive.</span></span>

## <a name="steps"></a><span data-ttu-id="8f18b-116">Schritte</span><span class="sxs-lookup"><span data-stu-id="8f18b-116">Steps</span></span>

<span data-ttu-id="8f18b-117">Das ASP.NET AJAX Control Toolkit enthält die MutuallyExclusiveCheckBox-Extender.</span><span class="sxs-lookup"><span data-stu-id="8f18b-117">The ASP.NET AJAX Control Toolkit contains the MutuallyExclusiveCheckBox extender.</span></span> <span data-ttu-id="8f18b-118">Programmierer, der Name einer Gruppe alle Kontrollkästchen zuweisen können (`Key` Attribut).</span><span class="sxs-lookup"><span data-stu-id="8f18b-118">This enables programmers to assign any checkbox to a group name (`Key` attribute).</span></span> <span data-ttu-id="8f18b-119">Aus alle Kontrollkästchen in der gleichen Gruppe kann nur eine gleichzeitig ausgewählt werden.</span><span class="sxs-lookup"><span data-stu-id="8f18b-119">From all check boxes within the same group, only one may be selected at one time.</span></span>

<span data-ttu-id="8f18b-120">Wir beginnen mit zwei Kontrollkästchen in eine neue ASP.NET-Seite zu platzieren.</span><span class="sxs-lookup"><span data-stu-id="8f18b-120">Let's start with putting two check boxes on a new ASP.NET page.</span></span> <span data-ttu-id="8f18b-121">Es können mehrere, aber zwei reicht aus, um das Prinzip zu demonstrieren:</span><span class="sxs-lookup"><span data-stu-id="8f18b-121">There can be more, but two of them suffice to demonstrate the principle:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample1.aspx)]

<span data-ttu-id="8f18b-122">Für beide Kontrollkästchen muss ein MutuallyExclusiveCheckBoxExtender-Steuerelement auf der Seite eingefügt werden.</span><span class="sxs-lookup"><span data-stu-id="8f18b-122">For both checkboxes, a MutuallyExclusiveCheckBoxExtender control must be put on the page.</span></span> <span data-ttu-id="8f18b-123">Beide Schlüsselattribute müssen den gleichen Wert wie der Wert sich Attribute eines HTML-Radio Button-Elemente befinden, müssen identisch mit die Gruppe zu bezeichnen, in der sie angehören.</span><span class="sxs-lookup"><span data-stu-id="8f18b-123">Both Key attributes need to have the same value, just as the value attributes of HTML radio button elements must be identical to denote the group they belong to.</span></span> <span data-ttu-id="8f18b-124">Die TargetControlID-Eigenschaft des Extenders verweist auf die ID des Kontrollkästchens.</span><span class="sxs-lookup"><span data-stu-id="8f18b-124">The TargetControlID property of the extender points to the ID of the check box.</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample2.aspx)]

<span data-ttu-id="8f18b-125">Darüber hinaus enthalten die ASP.NET AJAX `ScriptManager` die alle Elemente des ASP.NET AJAX Control Toolkit erforderlich ist:</span><span class="sxs-lookup"><span data-stu-id="8f18b-125">Finally, include the ASP.NET AJAX `ScriptManager` which is required by all elements of the ASP.NET AJAX Control Toolkit:</span></span>

[!code-aspx[Main](creating-mutually-exclusive-checkboxes-cs/samples/sample3.aspx)]

<span data-ttu-id="8f18b-126">Speichern, und führen Sie die Seite: Sie aktivieren und deaktivieren Sie beide Kontrollkästchen, können jedoch zu keinem Zeitpunkt beide Kontrollkästchen geprüft werden kann.</span><span class="sxs-lookup"><span data-stu-id="8f18b-126">Save and run the page: You can check and uncheck both check boxes, however at no time can both check boxes be checked.</span></span>


<span data-ttu-id="8f18b-127">[![Nur ein Kontrollkästchen kann zu einem Zeitpunkt überprüft werden](creating-mutually-exclusive-checkboxes-cs/_static/image2.png)](creating-mutually-exclusive-checkboxes-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8f18b-127">[![Only one checkbox can be checked at a time](creating-mutually-exclusive-checkboxes-cs/_static/image2.png)](creating-mutually-exclusive-checkboxes-cs/_static/image1.png)</span></span>

<span data-ttu-id="8f18b-128">Nur ein Kontrollkästchen kann zu einem Zeitpunkt überprüft werden ([klicken Sie, um das Bild in voller Größe anzeigen](creating-mutually-exclusive-checkboxes-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="8f18b-128">Only one checkbox can be checked at a time ([Click to view full-size image](creating-mutually-exclusive-checkboxes-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="8f18b-129">Nächste</span><span class="sxs-lookup"><span data-stu-id="8f18b-129">Next</span></span>](creating-mutually-exclusive-checkboxes-vb.md)
