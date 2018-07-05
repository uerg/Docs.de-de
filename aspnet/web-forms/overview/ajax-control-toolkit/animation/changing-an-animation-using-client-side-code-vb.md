---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
title: Ändern von Animationen mit clientseitigem Code (VB) | Microsoft-Dokumentation
author: wenz
description: Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Es können auch die Animation...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a7fe5de5-a964-4780-ae5e-70821dfb50a0
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: cc8ca2c962c5ebe5e0c45d5b575031ada3e64acd
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37386746"
---
<a name="changing-an-animation-using-client-side-code-vb"></a><span data-ttu-id="449d2-104">Ändern von Animationen mit clientseitigem Code (VB)</span><span class="sxs-lookup"><span data-stu-id="449d2-104">Changing an Animation Using Client-Side Code (VB)</span></span>
====================
<span data-ttu-id="449d2-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="449d2-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="449d2-106">[Code herunterladen](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.vb.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="449d2-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11VB.pdf)</span></span>

> <span data-ttu-id="449d2-107">Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="449d2-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="449d2-108">Die Animation kann auch mit benutzerdefinierte clientseitige JavaScript-Code geändert werden.</span><span class="sxs-lookup"><span data-stu-id="449d2-108">The animation can also be changed using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="449d2-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="449d2-109">Overview</span></span>

<span data-ttu-id="449d2-110">Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="449d2-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="449d2-111">Die Animation kann auch mit benutzerdefinierte clientseitige JavaScript-Code geändert werden.</span><span class="sxs-lookup"><span data-stu-id="449d2-111">The animation can also be changed using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="449d2-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="449d2-112">Steps</span></span>

<span data-ttu-id="449d2-113">Zunächst einmal sind die `ScriptManager` in die Seite klicken Sie dann die ASP.NET AJAX-Bibliothek wird geladen, lässt sich das Steuerelement-Toolkit verwenden:</span><span class="sxs-lookup"><span data-stu-id="449d2-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample1.aspx)]

<span data-ttu-id="449d2-114">Die Animation wird auf einen Bereich des Texts angewendet werden, der so aussieht:</span><span class="sxs-lookup"><span data-stu-id="449d2-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample2.aspx)]

<span data-ttu-id="449d2-115">Definieren Sie in der zugehörigen CSS-Klasse für den Bereich eine gute Hintergrundfarbe aus, und auch festlegen Sie eine feste Breite für den Bereich:</span><span class="sxs-lookup"><span data-stu-id="449d2-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](changing-an-animation-using-client-side-code-vb/samples/sample3.css)]

<span data-ttu-id="449d2-116">Die tatsächliche Animation wird durch eine HTML-Schaltfläche gestartet:</span><span class="sxs-lookup"><span data-stu-id="449d2-116">The actual animation is launched by an HTML button:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample4.aspx)]

<span data-ttu-id="449d2-117">Fügen Sie dann die `AnimationExtender` auf der Seite Bereitstellen einer `ID`, `TargetControlID` -Attribut und das obligatorische `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="449d2-117">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-vb/samples/sample5.aspx)]

<span data-ttu-id="449d2-118">Beachten Sie, dass es keine `<Animations>` Knoten innerhalb der `AnimationExtender` Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="449d2-118">Note that there is no `<Animations>` node within the `AnimationExtender` control.</span></span> <span data-ttu-id="449d2-119">Benutzerdefinierte JavaScript-Code wird verwendet, geben Sie die Animationen, die mit dem Steuerelement verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="449d2-119">Custom JavaScript code is used to provide the animations to be used with the control.</span></span>

<span data-ttu-id="449d2-120">Wie bei der Server-API von `AnimationExtender`, es gibt keine einfache Möglichkeit, eine Animation für den Extender noch nicht zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="449d2-120">As with the server API of `AnimationExtender`, there is no easy way to assign an animation to the extender yet.</span></span> <span data-ttu-id="449d2-121">Mit den verschiedenen Ereignissen registriert jedoch der Extender zur Verfügung stellt mehrere Methoden zum Lesen und Schreiben von Animationen (`OnClick`, `OnLoad`und so weiter).</span><span class="sxs-lookup"><span data-stu-id="449d2-121">However the extender does expose several methods to read and write animations registered with the various events (`OnClick`, `OnLoad`, and so on).</span></span> <span data-ttu-id="449d2-122">Hier einige Beispiele:</span><span class="sxs-lookup"><span data-stu-id="449d2-122">Here are some examples:</span></span>

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

<span data-ttu-id="449d2-123">Das Format des Rückgabewerts von der `get_*()` Funktionen und das Format des Arguments für die `set_*()` Funktionen ist eine JSON-Zeichenfolge, die eine objektdarstellung, der das XML-Markup bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="449d2-123">The format of the return value of the `get_*()` functions and the format of the argument for the `set_*()` functions is a JSON string, providing an object representation of what the XML markup would be.</span></span> <span data-ttu-id="449d2-124">Derzeit besteht keine Möglichkeit, ein Objekt übergeben, aber es ist möglich, ein Objekt aus einer angegebenen Animation lesen (`get_OnXXXBehavior()` Methoden).</span><span class="sxs-lookup"><span data-stu-id="449d2-124">Currently, there is no way to pass an object in, but it is possible to read an object from a given animation (`get_OnXXXBehavior()` methods).</span></span>

<span data-ttu-id="449d2-125">Hier ist eine JSON-Zeichenfolge (ohne die begrenzenden Anführungszeichen und ordentlich), die eine Animation, die von der Schaltfläche ausgelöste darstellt, aber die Animation des Bereichs durch Ändern der Größe und gleichzeitig ausblenden:</span><span class="sxs-lookup"><span data-stu-id="449d2-125">Here is a JSON string (without the delimiting quotes and formatted nicely) representing an animation triggered by the button, but animating the panel by resizing it and fading it out at the same time:</span></span>

[!code-json[Main](changing-an-animation-using-client-side-code-vb/samples/sample6.json)]

<span data-ttu-id="449d2-126">Die folgende JavaScript-Code weist diese JSON-descripting auf die `OnClick` Animation des aktuellen Extenders und führt ihn:</span><span class="sxs-lookup"><span data-stu-id="449d2-126">The following JavaScript code assigns this JSON descripting to the `OnClick` animation of the current extender and runs it:</span></span>

[!code-html[Main](changing-an-animation-using-client-side-code-vb/samples/sample7.html)]


<span data-ttu-id="449d2-127">[![Die Animation wird sofort ausgeführt, ohne einen Mausklick (und mit sehr geringem Markup)](changing-an-animation-using-client-side-code-vb/_static/image2.png)](changing-an-animation-using-client-side-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="449d2-127">[![The animation runs immediately, without a mouse click (and with very little markup)](changing-an-animation-using-client-side-code-vb/_static/image2.png)](changing-an-animation-using-client-side-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="449d2-128">Die Animation wird sofort ausgeführt, ohne ein Mausklick (und mit sehr geringem Markup) ([klicken Sie, um das Bild in voller Größe anzeigen](changing-an-animation-using-client-side-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="449d2-128">The animation runs immediately, without a mouse click (and with very little markup) ([Click to view full-size image](changing-an-animation-using-client-side-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="449d2-129">[Zurück](executing-animations-using-client-side-code-vb.md)
> [Weiter](animating-an-updatepanel-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="449d2-129">[Previous](executing-animations-using-client-side-code-vb.md)
[Next](animating-an-updatepanel-control-vb.md)</span></span>
