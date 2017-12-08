---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
title: Verwenden von mehreren Popup-Steuerelementen (c#) | Microsoft Docs
author: wenz
description: "Der Extender PopupControl im AJAX Control Toolkit bietet eine einfache Möglichkeit, um ein Popup auszulösen, wenn es sich bei jedem anderen Steuerelement aktiviert wird. Es ist auch möglich, verwenden Sie m..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 91511b0b-311d-481f-9e7c-73f07b813b79
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: d8c9742bb39b655115cb1862ff6e867989e63665
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="using-multiple-popup-controls-c"></a><span data-ttu-id="7c66d-104">Verwenden von mehreren Popup-Steuerelementen (c#)</span><span class="sxs-lookup"><span data-stu-id="7c66d-104">Using Multiple Popup Controls (C#)</span></span>
====================
<span data-ttu-id="7c66d-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="7c66d-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="7c66d-106">[Herunterladen von Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.cs.zip) oder [PDF herunterladen](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="7c66d-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1CS.pdf)</span></span>

> <span data-ttu-id="7c66d-107">Der Extender PopupControl im AJAX Control Toolkit bietet eine einfache Möglichkeit, um ein Popup auszulösen, wenn es sich bei jedem anderen Steuerelement aktiviert wird.</span><span class="sxs-lookup"><span data-stu-id="7c66d-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="7c66d-108">Es ist auch möglich, mehr als ein Popupsteuerelement auf einer Seite zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="7c66d-108">It is also possible to use more than one popup control on one page.</span></span>


## <a name="overview"></a><span data-ttu-id="7c66d-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="7c66d-109">Overview</span></span>

<span data-ttu-id="7c66d-110">Der Extender PopupControl im AJAX Control Toolkit bietet eine einfache Möglichkeit, um ein Popup auszulösen, wenn es sich bei jedem anderen Steuerelement aktiviert wird.</span><span class="sxs-lookup"><span data-stu-id="7c66d-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="7c66d-111">Es ist auch möglich, mehr als ein Popupsteuerelement auf einer Seite zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="7c66d-111">It is also possible to use more than one popup control on one page.</span></span>

## <a name="steps"></a><span data-ttu-id="7c66d-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="7c66d-112">Steps</span></span>

<span data-ttu-id="7c66d-113">Um die Funktionalität von ASP.NET AJAX und das Steuerelement-Toolkit aktivieren die `ScriptManager` Steuerelement an einer beliebigen Stelle auf der Seite versetzt werden muss (jedoch innerhalb der `<form>` Element):</span><span class="sxs-lookup"><span data-stu-id="7c66d-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample1.aspx)]

<span data-ttu-id="7c66d-114">Fügen Sie ein Panel die als das Popup dient.</span><span class="sxs-lookup"><span data-stu-id="7c66d-114">Next, add a panel which serves as the popup.</span></span> <span data-ttu-id="7c66d-115">In das aktuelle Szenario Bereich enthält eine `Calendar` Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="7c66d-115">In the current scenario, the panel contains a `Calendar` control.</span></span> <span data-ttu-id="7c66d-116">Um die Seite wird aktualisiert, die durch den Kalender Postbacks verursacht zu vermeiden, wird im Bereich eingefügt, innerhalb einer `UpdatePanel` Steuerelement:</span><span class="sxs-lookup"><span data-stu-id="7c66d-116">In order to avoid the page refreshes caused by the Calendar's postbacks, the panel is put within an `UpdatePanel` control:</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample2.aspx)]

<span data-ttu-id="7c66d-117">Die Seite enthält außerdem zwei Textfelder.</span><span class="sxs-lookup"><span data-stu-id="7c66d-117">The page also contains two text boxes.</span></span> <span data-ttu-id="7c66d-118">Für jedes Textfeld muss das Kalender-Popup angezeigt, wenn das Textfeld aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="7c66d-118">For each text box, the calendar popup shall appear once the text box is activated.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample3.aspx)]

<span data-ttu-id="7c66d-119">Jetzt erweitern Sie die beiden Textfelder mit einem `PopupControlExtender`.</span><span class="sxs-lookup"><span data-stu-id="7c66d-119">Now extend each of the two text boxes with a `PopupControlExtender`.</span></span> <span data-ttu-id="7c66d-120">Die `TargetControlID` Attribut stellt die ID des Steuerelements an den Extender gebunden sind.</span><span class="sxs-lookup"><span data-stu-id="7c66d-120">The `TargetControlID` attribute provides the ID of the control tied to the extender.</span></span> <span data-ttu-id="7c66d-121">Die `PopupControlID` Attribut enthält die ID der Popup-Fenster.</span><span class="sxs-lookup"><span data-stu-id="7c66d-121">The `PopupControlID` attribute contains the ID of the popup panel.</span></span> <span data-ttu-id="7c66d-122">In diesem Fall beide Extender anzeigen im gleichen Bereich, aber unterschiedliche Bereichen sind ebenfalls möglich.</span><span class="sxs-lookup"><span data-stu-id="7c66d-122">In this case, both extenders show the same panel, but different panels are possible, as well.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-cs/samples/sample4.aspx)]

<span data-ttu-id="7c66d-123">Jetzt sobald Sie in einem Textfeld klicken, erscheint ein Kalender unterhalb des Felds ein Datum auswählen.</span><span class="sxs-lookup"><span data-stu-id="7c66d-123">Now whenever you click within a text field, a calendar appears below the field, allowing you to select a date.</span></span> <span data-ttu-id="7c66d-124">(Beim Abrufen des ausgewählten Datums zurück in die Textfelder werden in einem anderen Lernprogramm behandelt.)</span><span class="sxs-lookup"><span data-stu-id="7c66d-124">(Getting the selected date back into the text boxes will be covered in a different tutorial.)</span></span>


<span data-ttu-id="7c66d-125">[![Der Kalender wird angezeigt, wenn der Benutzer in das Textfeld klickt](using-multiple-popup-controls-cs/_static/image2.png)](using-multiple-popup-controls-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7c66d-125">[![The Calendar appears when the user clicks into the textbox](using-multiple-popup-controls-cs/_static/image2.png)](using-multiple-popup-controls-cs/_static/image1.png)</span></span>

<span data-ttu-id="7c66d-126">Der Kalender wird angezeigt, wenn der Benutzer in das Textfeld klickt ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-multiple-popup-controls-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="7c66d-126">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](using-multiple-popup-controls-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="7c66d-127">Nächste</span><span class="sxs-lookup"><span data-stu-id="7c66d-127">Next</span></span>](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
