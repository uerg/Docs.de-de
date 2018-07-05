---
uid: web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
title: Datenbindung des Schieberegler-Steuerelements (c#) | Microsoft-Dokumentation
author: wenz
description: Das Schieberegler-Steuerelement im AJAX Control Toolkit stellt einen grafischen Schieberegler, der mit der Maus kontrolliert werden können. Es ist möglich, binden Sie die aktuelle Position...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: b7f77869-aa1d-4025-924f-622c57112db6
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/databinding-the-slider-control-cs
msc.type: authoredcontent
ms.openlocfilehash: cbb53309ccde9ed6be67a977a56cf2942bbe7f8c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37400654"
---
<a name="databinding-the-slider-control-c"></a><span data-ttu-id="339f6-104">Datenbindung des Schieberegler-Steuerelements (c#)</span><span class="sxs-lookup"><span data-stu-id="339f6-104">Databinding the Slider Control (C#)</span></span>
====================
<span data-ttu-id="339f6-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="339f6-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="339f6-106">[Code herunterladen](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.cs.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="339f6-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider0.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/slider0CS.pdf)</span></span>

> <span data-ttu-id="339f6-107">Das Schieberegler-Steuerelement im AJAX Control Toolkit stellt einen grafischen Schieberegler, der mit der Maus kontrolliert werden können.</span><span class="sxs-lookup"><span data-stu-id="339f6-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="339f6-108">Es ist möglich, die die aktuelle Position des Schiebereglers an ein anderes ASP.NET-Steuerelement zu binden.</span><span class="sxs-lookup"><span data-stu-id="339f6-108">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>


## <a name="overview"></a><span data-ttu-id="339f6-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="339f6-109">Overview</span></span>

<span data-ttu-id="339f6-110">Das Schieberegler-Steuerelement im AJAX Control Toolkit stellt einen grafischen Schieberegler, der mit der Maus kontrolliert werden können.</span><span class="sxs-lookup"><span data-stu-id="339f6-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="339f6-111">Es ist möglich, die die aktuelle Position des Schiebereglers an ein anderes ASP.NET-Steuerelement zu binden.</span><span class="sxs-lookup"><span data-stu-id="339f6-111">It is possible to bind the current position of the slider to another ASP.NET control.</span></span>

## <a name="steps"></a><span data-ttu-id="339f6-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="339f6-112">Steps</span></span>

<span data-ttu-id="339f6-113">Um die Funktionalität von ASP.NET AJAX und das Steuerelement-Toolkit, aktivieren die `ScriptManager` Steuerelement an einer beliebigen Stelle auf der Seite platziert werden muss (jedoch innerhalb der `<form>` Element):</span><span class="sxs-lookup"><span data-stu-id="339f6-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample1.aspx)]

<span data-ttu-id="339f6-114">Als Nächstes fügen Sie zwei `TextBox` Steuerelemente auf der Seite.</span><span class="sxs-lookup"><span data-stu-id="339f6-114">Next, add two `TextBox` controls to the page.</span></span> <span data-ttu-id="339f6-115">Transformiert eine in einen grafischen Schieberegler, und der andere Controller wird die Position des Schiebereglers aufzunehmen.</span><span class="sxs-lookup"><span data-stu-id="339f6-115">One will be transformed into a graphical slider, and the other one will hold the position of the slider.</span></span>

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample2.aspx)]

<span data-ttu-id="339f6-116">Der nächste Schritt ist bereits im letzten Schritt.</span><span class="sxs-lookup"><span data-stu-id="339f6-116">The next step is already the final step.</span></span> <span data-ttu-id="339f6-117">Die `SliderExtender` Steuerelement von ASP.NET AJAX Control Toolkit stellt einen Schieberegler aus dem ersten Textfeld, und das zweite Textfeld automatisch aktualisiert, wenn der Schieberegler positionieren Änderungen.</span><span class="sxs-lookup"><span data-stu-id="339f6-117">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit makes a slider out of the first text box and automatically updates the second text box when the slider position changes.</span></span> <span data-ttu-id="339f6-118">Damit dies funktioniert die `SliderExtender`des `TargetControlID` Attribut muss festgelegt werden, auf die ID des ersten Textfelds; die `BoundControlID` Attribut muss auf die ID des im zweiten Textfeld festgelegt werden.</span><span class="sxs-lookup"><span data-stu-id="339f6-118">In order for that to work, The `SliderExtender`'s `TargetControlID` attribute must be set to the ID of the first text box; the `BoundControlID` attribute must be set to the ID of the second text box.</span></span>

[!code-aspx[Main](databinding-the-slider-control-cs/samples/sample3.aspx)]

<span data-ttu-id="339f6-119">Wie Sie im Browser sehen können, funktioniert die Datenbindung in beide Richtungen: einen neuen Wert in das Textfeld eingeben, aktualisiert die Position der des Schiebereglers.</span><span class="sxs-lookup"><span data-stu-id="339f6-119">As you can see in the browser, the data binding works in both directions: entering a new value in the text box updates the slider's position.</span></span> <span data-ttu-id="339f6-120">Wenn Sie das zweite Textfeld schreibgeschützt machen, können Sie einen unzureichenden Schutz auf das Textfeld hinzufügen, sodass es schwieriger für den Benutzer manuell auf den Wert hier aktualisieren.</span><span class="sxs-lookup"><span data-stu-id="339f6-120">If you make the second text box read only, you may add a weak protection to the text field so that it is harder for the user to manually update the value in there.</span></span>


<span data-ttu-id="339f6-121">[![Schieberegler und Textfeld werden synchronisiert.](databinding-the-slider-control-cs/_static/image2.png)](databinding-the-slider-control-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="339f6-121">[![Slider and text box are in sync](databinding-the-slider-control-cs/_static/image2.png)](databinding-the-slider-control-cs/_static/image1.png)</span></span>

<span data-ttu-id="339f6-122">Schieberegler und Textfeld synchronisiert werden ([klicken Sie, um das Bild in voller Größe anzeigen](databinding-the-slider-control-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="339f6-122">Slider and text box are in sync ([Click to view full-size image](databinding-the-slider-control-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="339f6-123">[Zurück](using-the-slider-control-with-auto-postback-cs.md)
> [Weiter](using-the-slider-control-with-auto-postback-vb.md)</span><span class="sxs-lookup"><span data-stu-id="339f6-123">[Previous](using-the-slider-control-with-auto-postback-cs.md)
[Next](using-the-slider-control-with-auto-postback-vb.md)</span></span>
