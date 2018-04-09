---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
title: Bearbeiten von DropShadow-Eigenschaften aus dem Clientcode (VB) | Microsoft Docs
author: wenz
description: DropShadow-Steuerelements in der AJAX-Steuerelement-Toolkit erweitert einen Bereich mit Schatten. Eigenschaften von diesen Extender können auch mithilfe der Client JavaScrip geändert werden...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 11be4211-2fb9-4e15-b6d4-2aa623d81f3e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
msc.type: authoredcontent
ms.openlocfilehash: b5b024811ea511e67ce180169de9f0b7e3ef51d9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="manipulating-dropshadow-properties-from-client-code-vb"></a><span data-ttu-id="5e2cc-104">Bearbeiten von DropShadow-Eigenschaften aus dem Clientcode (VB)</span><span class="sxs-lookup"><span data-stu-id="5e2cc-104">Manipulating DropShadow Properties from Client Code (VB)</span></span>
====================
<span data-ttu-id="5e2cc-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="5e2cc-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="5e2cc-106">[Herunterladen von Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip) oder [PDF herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="5e2cc-106">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)</span></span>

> <span data-ttu-id="5e2cc-107">DropShadow-Steuerelements in der AJAX-Steuerelement-Toolkit erweitert einen Bereich mit Schatten.</span><span class="sxs-lookup"><span data-stu-id="5e2cc-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="5e2cc-108">Eigenschaften von diesen Extender können auch mit Client-JavaScript-Code geändert werden.</span><span class="sxs-lookup"><span data-stu-id="5e2cc-108">Properties of this extender can also be changed using client JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="5e2cc-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="5e2cc-109">Overview</span></span>

<span data-ttu-id="5e2cc-110">DropShadow-Steuerelements in der AJAX-Steuerelement-Toolkit erweitert einen Bereich mit Schatten.</span><span class="sxs-lookup"><span data-stu-id="5e2cc-110">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="5e2cc-111">Eigenschaften von diesen Extender können auch mit Client-JavaScript-Code geändert werden.</span><span class="sxs-lookup"><span data-stu-id="5e2cc-111">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="5e2cc-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="5e2cc-112">Steps</span></span>

<span data-ttu-id="5e2cc-113">Der Code beginnt mit einem Bereich, die einige Textzeilen enthält:</span><span class="sxs-lookup"><span data-stu-id="5e2cc-113">The code starts with a panel containing some lines of text:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample1.aspx)]

<span data-ttu-id="5e2cc-114">Zugeordnete CSS-Klasse bietet im Bereich eine gute Hintergrundfarbe:</span><span class="sxs-lookup"><span data-stu-id="5e2cc-114">The associated CSS class gives the panel a nice background color:</span></span>

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample2.css)]

<span data-ttu-id="5e2cc-115">Die `DropShadowExtender` wird hinzugefügt, um den Bereich mit einem Schlagschatteneffekt Deckkraft auf 50 % zu erweitern:</span><span class="sxs-lookup"><span data-stu-id="5e2cc-115">The `DropShadowExtender` is added to extend the panel with a drop shadow effect, opacity set to 50%:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample3.aspx)]

<span data-ttu-id="5e2cc-116">Klicken Sie dann, die ASP.NET AJAX `ScriptManager` Steuerelement ermöglicht das Toolkit Steuerelement funktioniert:</span><span class="sxs-lookup"><span data-stu-id="5e2cc-116">Then, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample4.aspx)]

<span data-ttu-id="5e2cc-117">Einen anderen Bereich enthält zwei JavaScript-Links für die Durchlässigkeit des ablageschattens: minus Link verringert die Deckkraft des Schattens, plus Link erhöht.</span><span class="sxs-lookup"><span data-stu-id="5e2cc-117">Another panel contains two JavaScript links for setting the opacity of the drop shadow: the minus link decreases the shadow's opacity, the plus link increases it.</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample5.aspx)]

<span data-ttu-id="5e2cc-118">Die JavaScript-Funktion `changeOpacity()` suchen müssen Sie dann zuerst die `DropShadowExtender` Steuerelement auf der Seite.</span><span class="sxs-lookup"><span data-stu-id="5e2cc-118">The JavaScript function `changeOpacity()` must then first find the `DropShadowExtender` control on the page.</span></span> <span data-ttu-id="5e2cc-119">ASP.NET AJAX-definiert die `$find()` Methode für genau diese Aufgabe.</span><span class="sxs-lookup"><span data-stu-id="5e2cc-119">ASP.NET AJAX defines the `$find()` method for exactly that task.</span></span> <span data-ttu-id="5e2cc-120">Anschließend wird die `get_Opacity()` Methode ruft die aktuellen Deckkraft, ab der `set_Opacity()` Methode legt sie fest.</span><span class="sxs-lookup"><span data-stu-id="5e2cc-120">Then, the `get_Opacity()` method retrieves the current opacity, the `set_Opacity()` method sets it.</span></span> <span data-ttu-id="5e2cc-121">Der JavaScript-Code fügt dann der aktuelle Wert für die Deckkraft in der `<label>` Element:</span><span class="sxs-lookup"><span data-stu-id="5e2cc-121">The JavaScript code then puts the current opacity value in the `<label>` element:</span></span>

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample6.html)]


<span data-ttu-id="5e2cc-122">[![Die Deckkraft wird auf der Clientseite geändert.](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="5e2cc-122">[![The opacity is changed on the client side](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="5e2cc-123">Die Deckkraft wird auf der Clientseite geändert ([klicken Sie hier, um das Bild in voller Größe angezeigt](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="5e2cc-123">The opacity is changed on the client side ([Click to view full-size image](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="5e2cc-124">Vorherige</span><span class="sxs-lookup"><span data-stu-id="5e2cc-124">Previous</span></span>](adjusting-the-z-index-of-a-dropshadow-vb.md)
