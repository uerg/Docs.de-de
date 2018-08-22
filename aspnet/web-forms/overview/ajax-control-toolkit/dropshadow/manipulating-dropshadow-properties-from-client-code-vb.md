---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
title: Bearbeiten von DropShadow-Eigenschaften über den Clientcode (VB) | Microsoft-Dokumentation
author: wenz
description: DropShadow-Steuerelements im AJAX Control Toolkit erweitert, einen Bereich mit einem Schlagschatteneffekt. Eigenschaften dieses Extenders können auch mit JavaScript-Client geändert werden...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 11be4211-2fb9-4e15-b6d4-2aa623d81f3e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-vb
msc.type: authoredcontent
ms.openlocfilehash: d8b8330174f6f49e96c42a0e15eeaf934bf24f87
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826992"
---
<a name="manipulating-dropshadow-properties-from-client-code-vb"></a><span data-ttu-id="c00d0-104">Bearbeiten von DropShadow-Eigenschaften über den Clientcode (VB)</span><span class="sxs-lookup"><span data-stu-id="c00d0-104">Manipulating DropShadow Properties from Client Code (VB)</span></span>
====================
<span data-ttu-id="c00d0-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c00d0-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c00d0-106">[Code herunterladen](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="c00d0-106">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2VB.pdf)</span></span>

> <span data-ttu-id="c00d0-107">DropShadow-Steuerelements im AJAX Control Toolkit erweitert, einen Bereich mit einem Schlagschatteneffekt.</span><span class="sxs-lookup"><span data-stu-id="c00d0-107">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="c00d0-108">Eigenschaften dieses Extenders können auch mithilfe von JavaScript-Clientcode geändert werden.</span><span class="sxs-lookup"><span data-stu-id="c00d0-108">Properties of this extender can also be changed using client JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="c00d0-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="c00d0-109">Overview</span></span>

<span data-ttu-id="c00d0-110">DropShadow-Steuerelements im AJAX Control Toolkit erweitert, einen Bereich mit einem Schlagschatteneffekt.</span><span class="sxs-lookup"><span data-stu-id="c00d0-110">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="c00d0-111">Eigenschaften dieses Extenders können auch mithilfe von JavaScript-Clientcode geändert werden.</span><span class="sxs-lookup"><span data-stu-id="c00d0-111">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="c00d0-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="c00d0-112">Steps</span></span>

<span data-ttu-id="c00d0-113">Der Code beginnt mit einem Bereich, der einige Textzeilen enthält:</span><span class="sxs-lookup"><span data-stu-id="c00d0-113">The code starts with a panel containing some lines of text:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample1.aspx)]

<span data-ttu-id="c00d0-114">Die zugeordnete CSS-Klasse bietet dem Bereich eine schöne Hintergrundfarbe:</span><span class="sxs-lookup"><span data-stu-id="c00d0-114">The associated CSS class gives the panel a nice background color:</span></span>

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample2.css)]

<span data-ttu-id="c00d0-115">Die `DropShadowExtender` hinzugefügt wird, erweitern Sie den Bereich mit einem Schlagschatteneffekt, Deckkraft, die auf 50 % festgelegt:</span><span class="sxs-lookup"><span data-stu-id="c00d0-115">The `DropShadowExtender` is added to extend the panel with a drop shadow effect, opacity set to 50%:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample3.aspx)]

<span data-ttu-id="c00d0-116">Klicken Sie dann das ASP.NET AJAX `ScriptManager` -Steuerelement können Sie das Steuerelement-Toolkit funktioniert:</span><span class="sxs-lookup"><span data-stu-id="c00d0-116">Then, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample4.aspx)]

<span data-ttu-id="c00d0-117">Einen anderen Bereich enthält zwei JavaScript-Links zum Festlegen der Durchlässigkeit des Schlagschattens: das Minuszeichen links verringert den Schatten Deckkraft, die plus-Verknüpfung wird vergrößert.</span><span class="sxs-lookup"><span data-stu-id="c00d0-117">Another panel contains two JavaScript links for setting the opacity of the drop shadow: the minus link decreases the shadow's opacity, the plus link increases it.</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample5.aspx)]

<span data-ttu-id="c00d0-118">Die JavaScript-Funktion `changeOpacity()` suchen müssen Sie dann zuerst die `DropShadowExtender` Steuerelement auf der Seite.</span><span class="sxs-lookup"><span data-stu-id="c00d0-118">The JavaScript function `changeOpacity()` must then first find the `DropShadowExtender` control on the page.</span></span> <span data-ttu-id="c00d0-119">ASP.NET AJAX-definiert den `$find()` -Methode für genau diese Aufgabe.</span><span class="sxs-lookup"><span data-stu-id="c00d0-119">ASP.NET AJAX defines the `$find()` method for exactly that task.</span></span> <span data-ttu-id="c00d0-120">Anschließend wird die `get_Opacity()` -Methode ruft die aktuelle Durchlässigkeit, ab der `set_Opacity()` Methode festgelegt.</span><span class="sxs-lookup"><span data-stu-id="c00d0-120">Then, the `get_Opacity()` method retrieves the current opacity, the `set_Opacity()` method sets it.</span></span> <span data-ttu-id="c00d0-121">Der JavaScript-Code fügt den aktuellen durchsichtigkeitswert klicken Sie dann in der `<label>` Element:</span><span class="sxs-lookup"><span data-stu-id="c00d0-121">The JavaScript code then puts the current opacity value in the `<label>` element:</span></span>

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-vb/samples/sample6.html)]


<span data-ttu-id="c00d0-122">[![Die Deckkraft wird auf der Clientseite geändert.](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c00d0-122">[![The opacity is changed on the client side](manipulating-dropshadow-properties-from-client-code-vb/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="c00d0-123">Die Deckkraft wird auf der Clientseite geändert ([klicken Sie, um das Bild in voller Größe anzeigen](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c00d0-123">The opacity is changed on the client side ([Click to view full-size image](manipulating-dropshadow-properties-from-client-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="c00d0-124">Vorherige</span><span class="sxs-lookup"><span data-stu-id="c00d0-124">Previous</span></span>](adjusting-the-z-index-of-a-dropshadow-vb.md)
