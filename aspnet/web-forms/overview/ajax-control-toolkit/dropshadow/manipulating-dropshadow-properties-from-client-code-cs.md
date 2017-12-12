---
uid: web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
title: Bearbeiten von DropShadow-Eigenschaften von Clientcode (c#) | Microsoft Docs
author: wenz
description: "Anpassen der DataList Bearbeitung der Benutzeroberfläche"
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: c83ca3e6-c0bf-4158-a166-40c1ab0f33da
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/dropshadow/manipulating-dropshadow-properties-from-client-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 59f7d4610ce610ef4357510f0e861f107278b5da
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="manipulating-dropshadow-properties-from-client-code-c"></a><span data-ttu-id="fe543-103">Bearbeiten von DropShadow-Eigenschaften von Clientcode (c#)</span><span class="sxs-lookup"><span data-stu-id="fe543-103">Manipulating DropShadow Properties from Client Code (C#)</span></span>
====================
<span data-ttu-id="fe543-104">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="fe543-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="fe543-105">[Herunterladen von Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) oder [PDF herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="fe543-105">[Download Code](http://download.microsoft.com/download/5/1/6/51652a81-500b-4f6b-88d3-617103e7941e/DropShadow2.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/dropshadow2CS.pdf)</span></span>

> <span data-ttu-id="fe543-106">DropShadow-Steuerelements in der AJAX-Steuerelement-Toolkit erweitert einen Bereich mit Schatten.</span><span class="sxs-lookup"><span data-stu-id="fe543-106">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="fe543-107">Eigenschaften von diesen Extender können auch mit Client-JavaScript-Code geändert werden.</span><span class="sxs-lookup"><span data-stu-id="fe543-107">Properties of this extender can also be changed using client JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="fe543-108">Übersicht</span><span class="sxs-lookup"><span data-stu-id="fe543-108">Overview</span></span>

<span data-ttu-id="fe543-109">DropShadow-Steuerelements in der AJAX-Steuerelement-Toolkit erweitert einen Bereich mit Schatten.</span><span class="sxs-lookup"><span data-stu-id="fe543-109">The DropShadow control in the AJAX Control Toolkit extends a panel with a drop shadow.</span></span> <span data-ttu-id="fe543-110">Eigenschaften von diesen Extender können auch mit Client-JavaScript-Code geändert werden.</span><span class="sxs-lookup"><span data-stu-id="fe543-110">Properties of this extender can also be changed using client JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="fe543-111">Schritte</span><span class="sxs-lookup"><span data-stu-id="fe543-111">Steps</span></span>

<span data-ttu-id="fe543-112">Der Code beginnt mit einem Bereich, die einige Textzeilen enthält:</span><span class="sxs-lookup"><span data-stu-id="fe543-112">The code starts with a panel containing some lines of text:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample1.aspx)]

<span data-ttu-id="fe543-113">Zugeordnete CSS-Klasse bietet im Bereich eine gute Hintergrundfarbe:</span><span class="sxs-lookup"><span data-stu-id="fe543-113">The associated CSS class gives the panel a nice background color:</span></span>

[!code-css[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample2.css)]

<span data-ttu-id="fe543-114">Die `DropShadowExtender` wird hinzugefügt, um den Bereich mit einem Schlagschatteneffekt Deckkraft auf 50 % zu erweitern:</span><span class="sxs-lookup"><span data-stu-id="fe543-114">The `DropShadowExtender` is added to extend the panel with a drop shadow effect, opacity set to 50%:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample3.aspx)]

<span data-ttu-id="fe543-115">Klicken Sie dann, die ASP.NET AJAX `ScriptManager` Steuerelement ermöglicht das Toolkit Steuerelement funktioniert:</span><span class="sxs-lookup"><span data-stu-id="fe543-115">Then, the ASP.NET AJAX `ScriptManager` control enables the Control Toolkit to work:</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample4.aspx)]

<span data-ttu-id="fe543-116">Einen anderen Bereich enthält zwei JavaScript-Links für die Durchlässigkeit des ablageschattens: minus Link verringert die Deckkraft des Schattens, plus Link erhöht.</span><span class="sxs-lookup"><span data-stu-id="fe543-116">Another panel contains two JavaScript links for setting the opacity of the drop shadow: the minus link decreases the shadow's opacity, the plus link increases it.</span></span>

[!code-aspx[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample5.aspx)]

<span data-ttu-id="fe543-117">Die JavaScript-Funktion `changeOpacity()` suchen müssen Sie dann zuerst die `DropShadowExtender` Steuerelement auf der Seite.</span><span class="sxs-lookup"><span data-stu-id="fe543-117">The JavaScript function `changeOpacity()` must then first find the `DropShadowExtender` control on the page.</span></span> <span data-ttu-id="fe543-118">ASP.NET AJAX-definiert die `$find()` Methode für genau diese Aufgabe.</span><span class="sxs-lookup"><span data-stu-id="fe543-118">ASP.NET AJAX defines the `$find()` method for exactly that task.</span></span> <span data-ttu-id="fe543-119">Anschließend wird die `get_Opacity()` Methode ruft die aktuellen Deckkraft, ab der `set_Opacity()` Methode legt sie fest.</span><span class="sxs-lookup"><span data-stu-id="fe543-119">Then, the `get_Opacity()` method retrieves the current opacity, the `set_Opacity()` method sets it.</span></span> <span data-ttu-id="fe543-120">Der JavaScript-Code fügt dann der aktuelle Wert für die Deckkraft in der `<label>` Element:</span><span class="sxs-lookup"><span data-stu-id="fe543-120">The JavaScript code then puts the current opacity value in the `<label>` element:</span></span>

[!code-html[Main](manipulating-dropshadow-properties-from-client-code-cs/samples/sample6.html)]


<span data-ttu-id="fe543-121">[![Die Deckkraft wird auf der Clientseite geändert.](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="fe543-121">[![The opacity is changed on the client side](manipulating-dropshadow-properties-from-client-code-cs/_static/image2.png)](manipulating-dropshadow-properties-from-client-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="fe543-122">Die Deckkraft wird auf der Clientseite geändert ([klicken Sie hier, um das Bild in voller Größe angezeigt](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="fe543-122">The opacity is changed on the client side ([Click to view full-size image](manipulating-dropshadow-properties-from-client-code-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="fe543-123">[Zurück](adjusting-the-z-index-of-a-dropshadow-cs.md)
[Weiter](adjusting-the-z-index-of-a-dropshadow-vb.md)</span><span class="sxs-lookup"><span data-stu-id="fe543-123">[Previous](adjusting-the-z-index-of-a-dropshadow-cs.md)
[Next](adjusting-the-z-index-of-a-dropshadow-vb.md)</span></span>
