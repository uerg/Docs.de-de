---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
title: Verwenden mehrere Popup-Steuerelementen (VB) | Microsoft Docs
author: wenz
description: Der Extender PopupControl im AJAX Control Toolkit bietet eine einfache Möglichkeit, um ein Popup auszulösen, wenn es sich bei jedem anderen Steuerelement aktiviert wird. Es ist auch möglich, verwenden Sie m...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4da43d77-f6c4-43a8-9124-f1e8e1c8f0a2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 7c57aab3ecf2c02a8488b5ea4e3e0ed33ac5e7fe
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30869321"
---
<a name="using-multiple-popup-controls-vb"></a><span data-ttu-id="50e44-104">Verwenden von mehreren Popup-Steuerelementen (VB)</span><span class="sxs-lookup"><span data-stu-id="50e44-104">Using Multiple Popup Controls (VB)</span></span>
====================
<span data-ttu-id="50e44-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="50e44-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="50e44-106">[Herunterladen von Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.vb.zip) oder [PDF herunterladen](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="50e44-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1VB.pdf)</span></span>

> <span data-ttu-id="50e44-107">Der Extender PopupControl im AJAX Control Toolkit bietet eine einfache Möglichkeit, um ein Popup auszulösen, wenn es sich bei jedem anderen Steuerelement aktiviert wird.</span><span class="sxs-lookup"><span data-stu-id="50e44-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="50e44-108">Es ist auch möglich, mehr als ein Popupsteuerelement auf einer Seite zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="50e44-108">It is also possible to use more than one popup control on one page.</span></span>


## <a name="overview"></a><span data-ttu-id="50e44-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="50e44-109">Overview</span></span>

<span data-ttu-id="50e44-110">Der Extender PopupControl im AJAX Control Toolkit bietet eine einfache Möglichkeit, um ein Popup auszulösen, wenn es sich bei jedem anderen Steuerelement aktiviert wird.</span><span class="sxs-lookup"><span data-stu-id="50e44-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="50e44-111">Es ist auch möglich, mehr als ein Popupsteuerelement auf einer Seite zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="50e44-111">It is also possible to use more than one popup control on one page.</span></span>

## <a name="steps"></a><span data-ttu-id="50e44-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="50e44-112">Steps</span></span>

<span data-ttu-id="50e44-113">Um die Funktionalität von ASP.NET AJAX und das Steuerelement-Toolkit aktivieren die `ScriptManager` Steuerelement an einer beliebigen Stelle auf der Seite versetzt werden muss (jedoch innerhalb der `<form>` Element):</span><span class="sxs-lookup"><span data-stu-id="50e44-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample1.aspx)]

<span data-ttu-id="50e44-114">Fügen Sie ein Panel die als das Popup dient.</span><span class="sxs-lookup"><span data-stu-id="50e44-114">Next, add a panel which serves as the popup.</span></span> <span data-ttu-id="50e44-115">In das aktuelle Szenario Bereich enthält eine `Calendar` Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="50e44-115">In the current scenario, the panel contains a `Calendar` control.</span></span> <span data-ttu-id="50e44-116">Um die Seite wird aktualisiert, die durch den Kalender Postbacks verursacht zu vermeiden, wird im Bereich eingefügt, innerhalb einer `UpdatePanel` Steuerelement:</span><span class="sxs-lookup"><span data-stu-id="50e44-116">In order to avoid the page refreshes caused by the Calendar's postbacks, the panel is put within an `UpdatePanel` control:</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample2.aspx)]

<span data-ttu-id="50e44-117">Die Seite enthält außerdem zwei Textfelder.</span><span class="sxs-lookup"><span data-stu-id="50e44-117">The page also contains two text boxes.</span></span> <span data-ttu-id="50e44-118">Für jedes Textfeld muss das Kalender-Popup angezeigt, wenn das Textfeld aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="50e44-118">For each text box, the calendar popup shall appear once the text box is activated.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample3.aspx)]

<span data-ttu-id="50e44-119">Jetzt erweitern Sie die beiden Textfelder mit einem `PopupControlExtender`.</span><span class="sxs-lookup"><span data-stu-id="50e44-119">Now extend each of the two text boxes with a `PopupControlExtender`.</span></span> <span data-ttu-id="50e44-120">Die `TargetControlID` Attribut stellt die ID des Steuerelements an den Extender gebunden sind.</span><span class="sxs-lookup"><span data-stu-id="50e44-120">The `TargetControlID` attribute provides the ID of the control tied to the extender.</span></span> <span data-ttu-id="50e44-121">Die `PopupControlID` Attribut enthält die ID der Popup-Fenster.</span><span class="sxs-lookup"><span data-stu-id="50e44-121">The `PopupControlID` attribute contains the ID of the popup panel.</span></span> <span data-ttu-id="50e44-122">In diesem Fall beide Extender anzeigen im gleichen Bereich, aber unterschiedliche Bereichen sind ebenfalls möglich.</span><span class="sxs-lookup"><span data-stu-id="50e44-122">In this case, both extenders show the same panel, but different panels are possible, as well.</span></span>

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample4.aspx)]

<span data-ttu-id="50e44-123">Jetzt sobald Sie in einem Textfeld klicken, erscheint ein Kalender unterhalb des Felds ein Datum auswählen.</span><span class="sxs-lookup"><span data-stu-id="50e44-123">Now whenever you click within a text field, a calendar appears below the field, allowing you to select a date.</span></span> <span data-ttu-id="50e44-124">(Beim Abrufen des ausgewählten Datums zurück in die Textfelder werden in einem anderen Lernprogramm behandelt.)</span><span class="sxs-lookup"><span data-stu-id="50e44-124">(Getting the selected date back into the text boxes will be covered in a different tutorial.)</span></span>


<span data-ttu-id="50e44-125">[![Der Kalender wird angezeigt, wenn der Benutzer in das Textfeld klickt](using-multiple-popup-controls-vb/_static/image2.png)](using-multiple-popup-controls-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="50e44-125">[![The Calendar appears when the user clicks into the textbox](using-multiple-popup-controls-vb/_static/image2.png)](using-multiple-popup-controls-vb/_static/image1.png)</span></span>

<span data-ttu-id="50e44-126">Der Kalender wird angezeigt, wenn der Benutzer in das Textfeld klickt ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-multiple-popup-controls-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="50e44-126">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](using-multiple-popup-controls-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="50e44-127">[Zurück](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
> [Weiter](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)</span><span class="sxs-lookup"><span data-stu-id="50e44-127">[Previous](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
[Next](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)</span></span>
