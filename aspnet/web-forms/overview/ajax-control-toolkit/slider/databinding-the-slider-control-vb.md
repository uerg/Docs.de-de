---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-vb
title: DataBinding das Schieberegler-Steuerelement (VB) | Microsoft Docs
author: wenz
description: Das Schieberegler-Steuerelement im AJAX Control Toolkit bietet einen grafische Schieberegler, der mit der Maus gesteuert werden kann. Es ist möglich, binden Sie die aktuelle Position...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4f3ba53f-d166-422d-b29c-403348057836
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 3ecd8598cd7fdcbbb4812e501bb30fa1f563a054
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879139"
---
<a name="databinding-the-slider-control-vb"></a><span data-ttu-id="7e3be-104">DataBinding das Schieberegler-Steuerelement (VB)</span><span class="sxs-lookup"><span data-stu-id="7e3be-104">Databinding the Slider Control (VB)</span></span>
====================
<span data-ttu-id="7e3be-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="7e3be-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="7e3be-106">[Herunterladen von Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.vb.zip) oder [PDF herunterladen](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="7e3be-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0VB.pdf)</span></span>

> <span data-ttu-id="7e3be-107">Das Schieberegler-Steuerelement im AJAX Control Toolkit bietet einen grafische Schieberegler, der mit der Maus gesteuert werden kann.</span><span class="sxs-lookup"><span data-stu-id="7e3be-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="7e3be-108">Es ist möglich, die aktuelle Position des Schiebereglers auf ein anderes ASP.NET-Steuerelement zu binden.</span><span class="sxs-lookup"><span data-stu-id="7e3be-108">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>


## <a name="overview"></a><span data-ttu-id="7e3be-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="7e3be-109">Overview</span></span>

<span data-ttu-id="7e3be-110">Das Schieberegler-Steuerelement im AJAX Control Toolkit bietet einen grafische Schieberegler, der mit der Maus gesteuert werden kann.</span><span class="sxs-lookup"><span data-stu-id="7e3be-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="7e3be-111">Es ist möglich, die aktuelle Position des Schiebereglers auf ein anderes ASP.NET-Steuerelement zu binden.</span><span class="sxs-lookup"><span data-stu-id="7e3be-111">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>

## <a name="steps"></a><span data-ttu-id="7e3be-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="7e3be-112">Steps</span></span>

<span data-ttu-id="7e3be-113">Um die Funktionalität von ASP.NET AJAX und das Steuerelement-Toolkit aktivieren die `ScriptManager` Steuerelement an einer beliebigen Stelle auf der Seite versetzt werden muss (jedoch innerhalb der `<form>` Element):</span><span class="sxs-lookup"><span data-stu-id="7e3be-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample1.aspx)]

<span data-ttu-id="7e3be-114">Als Nächstes fügen Sie zwei `TextBox` Steuerelemente auf der Seite.</span><span class="sxs-lookup"><span data-stu-id="7e3be-114">Next, add two `TextBox` controls to the page.</span></span> <span data-ttu-id="7e3be-115">Eine wird in einem grafischen Schieberegler transformiert werden, und der andere Controller wird die Position des Schiebereglers halten.</span><span class="sxs-lookup"><span data-stu-id="7e3be-115">One will be transformed into a graphical slider, and the other one will hold the position of the slider.</span></span>

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample2.aspx)]

<span data-ttu-id="7e3be-116">Der nächste Schritt ist bereits im letzten Schritt.</span><span class="sxs-lookup"><span data-stu-id="7e3be-116">The next step is already the final step.</span></span> <span data-ttu-id="7e3be-117">Die `SliderExtender` Steuerelement aus dem ASP.NET AJAX-Steuerelement-Toolkit stellt einen Schieberegler aus dem ersten Textfeld und das zweite Textfeld automatisch aktualisiert, wenn Änderungen positionieren Sie der Schieberegler.</span><span class="sxs-lookup"><span data-stu-id="7e3be-117">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit makes a slider out of the first text box and automatically updates the second text box when the slider position changes.</span></span> <span data-ttu-id="7e3be-118">Damit dies funktioniert die `SliderExtender`des `TargetControlID` Attribut muss festgelegt werden, auf die ID der im ersten Textfeld; die `BoundControlID` Attribut muss festgelegt werden, um die ID des zweiten Textfeld.</span><span class="sxs-lookup"><span data-stu-id="7e3be-118">In order for that to work, The `SliderExtender`'s `TargetControlID` attribute must be set to the ID of the first text box; the `BoundControlID` attribute must be set to the ID of the second text box.</span></span>

[!code-aspx[Main](databinding-the-slider-control-vb/samples/sample3.aspx)]

<span data-ttu-id="7e3be-119">Wie Sie im Browser sehen können, funktioniert die Datenbindung in beide Richtungen: Position des Schiebereglers aktualisiert einen neuen Wert in das Textfeld eingeben.</span><span class="sxs-lookup"><span data-stu-id="7e3be-119">As you can see in the browser, the data binding works in both directions: entering a new value in the text box updates the slider's position.</span></span> <span data-ttu-id="7e3be-120">Wenn Sie das zweite Textfeld schreibgeschützt vornehmen, können Sie das Textfeld "einen unzureichenden Schutz hinzufügen, damit es erschwert, sich für den Benutzer so aktualisieren Sie manuell auf den Wert vorhanden ist.</span><span class="sxs-lookup"><span data-stu-id="7e3be-120">If you make the second text box read only, you may add a weak protection to the text field so that it is harder for the user to manually update the value in there.</span></span>


<span data-ttu-id="7e3be-121">[![Schieberegler und Textfeld sind synchronisiert.](databinding-the-slider-control-vb/_static/image2.png)](databinding-the-slider-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7e3be-121">[![Slider and text box are in sync](databinding-the-slider-control-vb/_static/image2.png)](databinding-the-slider-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="7e3be-122">Schieberegler und Textfeld sind synchron ([klicken Sie hier, um das Bild in voller Größe angezeigt](databinding-the-slider-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="7e3be-122">Slider and text box are in sync ([Click to view full-size image](databinding-the-slider-control-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="7e3be-123">Vorherige</span><span class="sxs-lookup"><span data-stu-id="7e3be-123">Previous</span></span>](using-the-slider-control-with-auto-postback-vb.md)
