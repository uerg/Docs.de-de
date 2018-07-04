---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
title: Anpassen des Z-Index eines DropShadow-Steuerelements (c#) | Microsoft-Dokumentation
author: wenz
description: DropShadow-Steuerelements im AJAX Control Toolkit erweitert, einen Bereich mit einem Schlagschatteneffekt. Aber diese Schatten manchmal mit anderen Steuerelementen, für die Insta steht in Konflikt...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 14133833-e518-4347-87b9-6b6f71f14a77
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/adjusting-the-z-index-of-a-dropshadow-cs
msc.type: authoredcontent
ms.openlocfilehash: 22732bc44a56819a2ec73346dc32659d027eaee0
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37369569"
---
<a name="adjusting-the-z-index-of-a-dropshadow-c"></a><span data-ttu-id="1376d-104">Anpassen des Z-Index eines DropShadow-Steuerelements (c#)</span><span class="sxs-lookup"><span data-stu-id="1376d-104">Adjusting the Z-Index of a DropShadow (C#)</span></span>
====================
<span data-ttu-id="1376d-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="1376d-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="1376d-106">[Code herunterladen](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.cs.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="1376d-106">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow1.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow1CS.pdf)</span></span>

> <span data-ttu-id="1376d-107">DropShadow-Steuerelements im AJAX Control Toolkit erweitert, einen Bereich mit einem Schlagschatteneffekt.</span><span class="sxs-lookup"><span data-stu-id="1376d-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="1376d-108">Aber diese Schatten manchmal mit anderen Steuerelementen, z. B. das ASP.NET Menüsteuerelement in Konflikt steht.</span><span class="sxs-lookup"><span data-stu-id="1376d-108">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="1376d-109">Wenn ein Menüeintrag, holt es hinter dem Schlagschatten angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="1376d-109">When a menu entry pops up, it appears behind the drop shadow.</span></span>


## <a name="overview"></a><span data-ttu-id="1376d-110">Übersicht</span><span class="sxs-lookup"><span data-stu-id="1376d-110">Overview</span></span>

<span data-ttu-id="1376d-111">DropShadow-Steuerelements im AJAX Control Toolkit erweitert, einen Bereich mit einem Schlagschatteneffekt.</span><span class="sxs-lookup"><span data-stu-id="1376d-111">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="1376d-112">Aber diese Schatten manchmal mit anderen Steuerelementen, z. B. das ASP.NET Menüsteuerelement in Konflikt steht.</span><span class="sxs-lookup"><span data-stu-id="1376d-112">However this shadow sometimes conflicts with other controls, for instance the ASP.NET Menu control.</span></span> <span data-ttu-id="1376d-113">Wenn ein Menüeintrag, holt es hinter dem Schlagschatten angezeigt wird.</span><span class="sxs-lookup"><span data-stu-id="1376d-113">When a menu entry pops up, it appears behind the drop shadow.</span></span>

## <a name="steps"></a><span data-ttu-id="1376d-114">Schritte</span><span class="sxs-lookup"><span data-stu-id="1376d-114">Steps</span></span>

<span data-ttu-id="1376d-115">Der Code beginnt mit das Panel selbst, die viel Text enthält, damit der Bereich wenig Text für die Auswirkungen angezeigt werden, enthält:</span><span class="sxs-lookup"><span data-stu-id="1376d-115">The code commences with the Panel itself, containing enough text so that the panel contains enough text for the effect to be visible:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample1.aspx)]

<span data-ttu-id="1376d-116">Ein anderer Bereich befindet sich direkt vor der `panelShadow` Bereich.</span><span class="sxs-lookup"><span data-stu-id="1376d-116">Another panel is placed directly before the `panelShadow` panel.</span></span> <span data-ttu-id="1376d-117">Es enthält ein Menü mit der horizontalen Ausrichtung, sodass Menüeinträge über angezeigt (oder besser: unter) dem `dropShadow` Panel):</span><span class="sxs-lookup"><span data-stu-id="1376d-117">It contains a menu with horizontal orientation so that menu entries appear over (or rather: under) the `dropShadow` panel):</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample2.aspx)]

<span data-ttu-id="1376d-118">Anschließend wird die `DropShadowExtender` hinzugefügt wird, zum Erweitern der `panelShadow` Bereich mit einem Schlagschatteneffekt:</span><span class="sxs-lookup"><span data-stu-id="1376d-118">Then, the `DropShadowExtender` is added to extend the `panelShadow` panel with a drop shadow effect:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample3.aspx)]

<span data-ttu-id="1376d-119">Zum Schluss das ASP.NET AJAX `ScriptManager` -Steuerelement können Sie das Steuerelement-Toolkit funktioniert:</span><span class="sxs-lookup"><span data-stu-id="1376d-119">Finally, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample4.aspx)]

<span data-ttu-id="1376d-120">Wenn Sie dieses Skript ausführen, werden die Menüeinträge im unter dem Bereich angezeigt.</span><span class="sxs-lookup"><span data-stu-id="1376d-120">When you run this script, the menu entries appear underneath the panel.</span></span> <span data-ttu-id="1376d-121">Klicken Sie im Menü die CSS-Klasse verwendet, jedoch `panel` , in dem Sie müssen lediglich definieren Sie zwei Dinge besteht aus Elementen, die vor den anderen Fensterbereich angezeigt werden:</span><span class="sxs-lookup"><span data-stu-id="1376d-121">However the menu uses the CSS class `panel` where you just have to define two things to make elements appear in front of the other panel:</span></span>

- <span data-ttu-id="1376d-122">Relative Positionierung</span><span class="sxs-lookup"><span data-stu-id="1376d-122">Relative positioning</span></span>
- <span data-ttu-id="1376d-123">Eine positive Z-index</span><span class="sxs-lookup"><span data-stu-id="1376d-123">A positive z-index</span></span>

[!code-css[Main](adjusting-the-z-index-of-a-dropshadow-cs/samples/sample5.css)]

<span data-ttu-id="1376d-124">Anschließend wird die `DropShadowExtender` Steuerelement keine Konflikte mehr mit den Menu-Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="1376d-124">Then, the `DropShadowExtender` control does not conflict any longer with the Menu control.</span></span>


<span data-ttu-id="1376d-125">[![Vorher: Der Eintrag ist nicht sichtbar](adjusting-the-z-index-of-a-dropshadow-cs/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="1376d-125">[![Before: The menu entry is not visible](adjusting-the-z-index-of-a-dropshadow-cs/_static/image2.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image1.png)</span></span>

<span data-ttu-id="1376d-126">Zuvor: Ist der Eintrag nicht sichtbar ([klicken Sie, um das Bild in voller Größe anzeigen](adjusting-the-z-index-of-a-dropshadow-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="1376d-126">Before: The menu entry is not visible ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-cs/_static/image3.png))</span></span>


<span data-ttu-id="1376d-127">[![Nachher: Der Eintrag im Menü angezeigt wird](adjusting-the-z-index-of-a-dropshadow-cs/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="1376d-127">[![After: The menu entry appears](adjusting-the-z-index-of-a-dropshadow-cs/_static/image5.png)](adjusting-the-z-index-of-a-dropshadow-cs/_static/image4.png)</span></span>

<span data-ttu-id="1376d-128">Nachher: Der Eintrag im Menü angezeigt wird ([klicken Sie, um das Bild in voller Größe anzeigen](adjusting-the-z-index-of-a-dropshadow-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="1376d-128">After: The menu entry appears ([Click to view full-size image](adjusting-the-z-index-of-a-dropshadow-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="1376d-129">Nächste</span><span class="sxs-lookup"><span data-stu-id="1376d-129">Next</span></span>](manipulating-dropshadow-properties-from-client-code-cs.md)
