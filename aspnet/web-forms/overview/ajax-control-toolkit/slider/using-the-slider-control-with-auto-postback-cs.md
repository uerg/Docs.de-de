---
uid: web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
title: Verwenden das Schieberegler-Steuerelement mit Auto-Postback (c#) | Microsoft Docs
author: wenz
description: Das Schieberegler-Steuerelement im AJAX Control Toolkit bietet einen grafische Schieberegler, der mit der Maus gesteuert werden kann. Es ist möglich, stellen die Schieberegler Autopost...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4d85e9fb-91e6-41f2-9c13-754549b19c27
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/slider/using-the-slider-control-with-auto-postback-cs
msc.type: authoredcontent
ms.openlocfilehash: e347d20c5c2ee48e6ed801e95459af6f0bcd2667
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="using-the-slider-control-with-auto-postback-c"></a><span data-ttu-id="672b9-104">Verwenden das Schieberegler-Steuerelement mit Auto-Postback (c#)</span><span class="sxs-lookup"><span data-stu-id="672b9-104">Using the Slider Control With Auto-Postback (C#)</span></span>
====================
<span data-ttu-id="672b9-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="672b9-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="672b9-106">[Herunterladen von Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip) oder [PDF herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="672b9-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/Slider1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/slider1CS.pdf)</span></span>

> <span data-ttu-id="672b9-107">Das Schieberegler-Steuerelement im AJAX Control Toolkit bietet einen grafische Schieberegler, der mit der Maus gesteuert werden kann.</span><span class="sxs-lookup"><span data-stu-id="672b9-107">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="672b9-108">Es ist möglich, stellen Sie der Schieberegler Autopostback einmal dessen Wert sich ändert.</span><span class="sxs-lookup"><span data-stu-id="672b9-108">It is possible to make the slider autopostback once its value changes.</span></span>


## <a name="overview"></a><span data-ttu-id="672b9-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="672b9-109">Overview</span></span>

<span data-ttu-id="672b9-110">Das Schieberegler-Steuerelement im AJAX Control Toolkit bietet einen grafische Schieberegler, der mit der Maus gesteuert werden kann.</span><span class="sxs-lookup"><span data-stu-id="672b9-110">The Slider control in the AJAX Control Toolkit provides a graphical slider that can be controlled using the mouse.</span></span> <span data-ttu-id="672b9-111">Es ist möglich, stellen Sie der Schieberegler Autopostback einmal dessen Wert sich ändert.</span><span class="sxs-lookup"><span data-stu-id="672b9-111">It is possible to make the slider autopostback once its value changes.</span></span>

## <a name="steps"></a><span data-ttu-id="672b9-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="672b9-112">Steps</span></span>

<span data-ttu-id="672b9-113">Um den Schieberegler nach einer Änderung automatisch postback machen, benötigen beide Textfelder Attributs `AutoPostBack="true"`: das Textfeld, das den Schieberegler selbst werden soll, und das Textfeld, das den Schieberegler Position enthält.</span><span class="sxs-lookup"><span data-stu-id="672b9-113">In order to make the slider automatically postback upon a change, both text boxes need the attribute `AutoPostBack="true"`: The text box that will become the slider itself, and the text box that holds the slider's position.</span></span> <span data-ttu-id="672b9-114">Hier ist das erforderliche Markup für diese:</span><span class="sxs-lookup"><span data-stu-id="672b9-114">Here is the required markup for that:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample1.aspx)]

<span data-ttu-id="672b9-115">Die `SliderExtender` Steuerelement aus dem ASP.NET AJAX-Steuerelement-Toolkit weist die beiden Textfelder die Schieberegler-Funktionalität:</span><span class="sxs-lookup"><span data-stu-id="672b9-115">The `SliderExtender` control from the ASP.NET AJAX Control Toolkit assigns the slider functionality to the two text boxes:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample2.aspx)]

<span data-ttu-id="672b9-116">Zusätzliche Label-Element wird später verwendet werden, um die Benutzer über ein Postback zu informieren:</span><span class="sxs-lookup"><span data-stu-id="672b9-116">An additional label element will later be used to inform the user of a postback:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample3.aspx)]

<span data-ttu-id="672b9-117">Schließlich die `ScriptManager` -Steuerelement von ASP.NET AJAX lädt die erforderlichen JavaScript für das Steuerelement Toolkit arbeiten:</span><span class="sxs-lookup"><span data-stu-id="672b9-117">Finally, the `ScriptManager` control of ASP.NET AJAX loads the required JavaScript for the Control Toolkit to work:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample4.aspx)]

<span data-ttu-id="672b9-118">Der Schieberegler ist jetzt wieder buchen; auf der Serverseite kann dieses Ereignis abgefangen und Reaktionen bewirkten:</span><span class="sxs-lookup"><span data-stu-id="672b9-118">Now the slider is posting back; on the server-side, this event may be caught and acted upon:</span></span>

[!code-aspx[Main](using-the-slider-control-with-auto-postback-cs/samples/sample5.aspx)]


<span data-ttu-id="672b9-119">[![Verschieben des Schiebereglers wird einen Postback ausgelöst.](using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="672b9-119">[![Moving the slider triggers a postback](using-the-slider-control-with-auto-postback-cs/_static/image2.png)](using-the-slider-control-with-auto-postback-cs/_static/image1.png)</span></span>

<span data-ttu-id="672b9-120">Verschieben des Schiebereglers wird einen Postback ausgelöst ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-the-slider-control-with-auto-postback-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="672b9-120">Moving the slider triggers a postback ([Click to view full-size image](using-the-slider-control-with-auto-postback-cs/_static/image3.png))</span></span>


<span data-ttu-id="672b9-121">[![Das Datum der Änderung wird anschließend in die Bezeichnung geschrieben.](using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="672b9-121">[![Afterwards, the date of this change is written in the label](using-the-slider-control-with-auto-postback-cs/_static/image5.png)](using-the-slider-control-with-auto-postback-cs/_static/image4.png)</span></span>

<span data-ttu-id="672b9-122">Danach wird das Datum der Änderung in die Bezeichnung geschrieben ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-the-slider-control-with-auto-postback-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="672b9-122">Afterwards, the date of this change is written in the label ([Click to view full-size image](using-the-slider-control-with-auto-postback-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="672b9-123">Nächste</span><span class="sxs-lookup"><span data-stu-id="672b9-123">Next</span></span>](databinding-the-slider-control-cs.md)
