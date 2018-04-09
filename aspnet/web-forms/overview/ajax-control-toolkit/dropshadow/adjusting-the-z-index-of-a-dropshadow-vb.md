---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
title: Anpassen der Z-Index von einem DropShadow (VB) | Microsoft Docs
author: wenz
description: DropShadow-Steuerelements in der AJAX-Steuerelement-Toolkit erweitert einen Bereich mit Schatten. Jedoch diese Schatten manchmal mit anderen Steuerelementen für Insta steht in Konflikt...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: ecb004b5-82c0-44fb-bcaf-233fffac6195
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-vb
msc.type: authoredcontent
ms.openlocfilehash: b484dc6bfa6f67bd6b70f7c36c2eb2ec7143edaf
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="adjusting-the-z-index-of-a-dropshadow-vb"></a><span data-ttu-id="c7e6e-104">Anpassen der Z-Index von einem DropShadow (VB)</span><span class="sxs-lookup"><span data-stu-id="c7e6e-104">Adjusting the Z-Index of a DropShadow (VB)</span></span>
====================
<span data-ttu-id="c7e6e-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c7e6e-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c7e6e-106">[Herunterladen von Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.vb.zip) oder [PDF herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="c7e6e-106">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1VB.pdf)</span></span>

> <span data-ttu-id="c7e6e-107">DropShadow-Steuerelements in der AJAX-Steuerelement-Toolkit erweitert einen Bereich mit Schatten.</span><span class="sxs-lookup"><span data-stu-id="c7e6e-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="c7e6e-108">Jedoch diese Schatten manchmal mit anderen Steuerelementen, z. B. das ASP.NET Menüsteuerelement steht in Konflikt.</span><span class="sxs-lookup"><span data-stu-id="c7e6e-108">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="c7e6e-109">Wenn ein Menüeintrag gestartet wurde, wird er hinter der Schatten angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="c7e6e-109">When a menu entry pops up, it appears behind the drop shadow.</span></span>


## <a name="overview"></a><span data-ttu-id="c7e6e-110">Übersicht</span><span class="sxs-lookup"><span data-stu-id="c7e6e-110">Overview</span></span>

<span data-ttu-id="c7e6e-111">DropShadow-Steuerelements in der AJAX-Steuerelement-Toolkit erweitert einen Bereich mit Schatten.</span><span class="sxs-lookup"><span data-stu-id="c7e6e-111">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="c7e6e-112">Jedoch diese Schatten manchmal mit anderen Steuerelementen, z. B. das ASP.NET Menüsteuerelement steht in Konflikt.</span><span class="sxs-lookup"><span data-stu-id="c7e6e-112">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="c7e6e-113">Wenn ein Menüeintrag gestartet wurde, wird er hinter der Schatten angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="c7e6e-113">When a menu entry pops up, it appears behind the drop shadow.</span></span>

## <a name="steps"></a><span data-ttu-id="c7e6e-114">Schritte</span><span class="sxs-lookup"><span data-stu-id="c7e6e-114">Steps</span></span>

<span data-ttu-id="c7e6e-115">Der Code beginnt mit dem Bildschirm selbst, genug Text enthält, sodass Bereich genug Text für den Effekt sichtbar sein enthält:</span><span class="sxs-lookup"><span data-stu-id="c7e6e-115">The code commences with the Panel itself, containing enough text so that the panel contains enough text for the effect to be visible:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample1.aspx)]

<span data-ttu-id="c7e6e-116">Einem anderen Bereich befindet sich direkt vor der `panelShadow` Bereich.</span><span class="sxs-lookup"><span data-stu-id="c7e6e-116">Another panel is placed directly before the `panelShadow` panel.</span></span> <span data-ttu-id="c7e6e-117">Enthält ein Menü mit horizontalen Ausrichtung, sodass über Menüeinträge im angezeigt werden (oder vielmehr: unter) die `dropShadow` Bereich):</span><span class="sxs-lookup"><span data-stu-id="c7e6e-117">It contains a menu with horizontal orientation so that menu entries appear over (or rather: under) the `dropShadow` panel):</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample2.aspx)]

<span data-ttu-id="c7e6e-118">Anschließend wird die `DropShadowExtender` wird hinzugefügt, zum Erweitern der `panelShadow` Bereich mit einem Schlagschatteneffekt:</span><span class="sxs-lookup"><span data-stu-id="c7e6e-118">Then, the `DropShadowExtender` is added to extend the `panelShadow` panel with a drop shadow effect:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample3.aspx)]

<span data-ttu-id="c7e6e-119">Schließlich ASP.NET AJAX `ScriptManager` Steuerelement ermöglicht das Toolkit Steuerelement funktioniert:</span><span class="sxs-lookup"><span data-stu-id="c7e6e-119">Finally, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample4.aspx)]

<span data-ttu-id="c7e6e-120">Wenn Sie dieses Skript ausführen, werden die Menüeinträge im unterhalb der Systemsteuerung angezeigt.</span><span class="sxs-lookup"><span data-stu-id="c7e6e-120">When you run this script, the menu entries appear underneath the panel.</span></span> <span data-ttu-id="c7e6e-121">Klicken Sie im Menü die CSS-Klasse verwendet, jedoch `panel` , in dem Sie lassen, definieren Sie zwei Dinge Elemente vor anderen Fensterbereich angezeigt:</span><span class="sxs-lookup"><span data-stu-id="c7e6e-121">However the menu uses the CSS class `panel` where you just have to define two things to make elements appear in front of the other panel:</span></span>

- <span data-ttu-id="c7e6e-122">Relative Positionierung</span><span class="sxs-lookup"><span data-stu-id="c7e6e-122">Relative positioning</span></span>
- <span data-ttu-id="c7e6e-123">Eine positive Z-index</span><span class="sxs-lookup"><span data-stu-id="c7e6e-123">A positive z-index</span></span>

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-vb/samples/sample5.css)]

<span data-ttu-id="c7e6e-124">Anschließend wird die `DropShadowExtender` Steuerelement keine Konflikte mehr mit den Menu-Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="c7e6e-124">Then, the `DropShadowExtender` control does not conflict any longer with the Menu control.</span></span>


<span data-ttu-id="c7e6e-125">[![Vorher: Die Menüeintrag ist nicht sichtbar](adjusting-the-z-index-of-a-dropshadow-vb/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c7e6e-125">[![Before: The menu entry is not visible](adjusting-the-z-index-of-a-dropshadow-vb/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image1.png)</span></span>

<span data-ttu-id="c7e6e-126">Vorher: Ist der Menüeintrag nicht sichtbar ([klicken Sie hier, um das Bild in voller Größe angezeigt](adjusting-the-z-index-of-a-dropshadow-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c7e6e-126">Before: The menu entry is not visible ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-vb/_static/image3.png))</span></span>


<span data-ttu-id="c7e6e-127">[![Nach: Der Menüeintrag im angezeigt wird](adjusting-the-z-index-of-a-dropshadow-vb/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="c7e6e-127">[![After: The menu entry appears](adjusting-the-z-index-of-a-dropshadow-vb/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-vb/_static/image4.png)</span></span>

<span data-ttu-id="c7e6e-128">Nach: Wird angezeigt, den Menüeintrag ([klicken Sie hier, um das Bild in voller Größe angezeigt](adjusting-the-z-index-of-a-dropshadow-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="c7e6e-128">After: The menu entry appears ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-vb/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="c7e6e-129">[Zurück](manipulating-dropshadow-properties-from-client-code-cs.md)
> [Weiter](manipulating-dropshadow-properties-from-client-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="c7e6e-129">[Previous](manipulating-dropshadow-properties-from-client-code-cs.md)
[Next](manipulating-dropshadow-properties-from-client-code-vb.md)</span></span>
