---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
title: Ändern von Animationen mit clientseitigen Code (c#) | Microsoft-Dokumentation
author: wenz
description: Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Es können auch die Animation...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 2bfbc5cc-f942-44b7-a62d-a29520f1da9a
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 9711e93ee6f119ec1825e32bdad64535435970c9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808955"
---
<a name="changing-an-animation-using-client-side-code-c"></a><span data-ttu-id="9399a-104">Ändern von Animationen mit clientseitigen Code (c#)</span><span class="sxs-lookup"><span data-stu-id="9399a-104">Changing an Animation Using Client-Side Code (C#)</span></span>
====================
<span data-ttu-id="9399a-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="9399a-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="9399a-106">[Code herunterladen](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.cs.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="9399a-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11CS.pdf)</span></span>

> <span data-ttu-id="9399a-107">Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="9399a-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="9399a-108">Die Animation kann auch mit benutzerdefinierte clientseitige JavaScript-Code geändert werden.</span><span class="sxs-lookup"><span data-stu-id="9399a-108">The animation can also be changed using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="9399a-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="9399a-109">Overview</span></span>

<span data-ttu-id="9399a-110">Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="9399a-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="9399a-111">Die Animation kann auch mit benutzerdefinierte clientseitige JavaScript-Code geändert werden.</span><span class="sxs-lookup"><span data-stu-id="9399a-111">The animation can also be changed using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="9399a-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="9399a-112">Steps</span></span>

<span data-ttu-id="9399a-113">Zunächst einmal sind die `ScriptManager` in die Seite klicken Sie dann die ASP.NET AJAX-Bibliothek wird geladen, lässt sich das Steuerelement-Toolkit verwenden:</span><span class="sxs-lookup"><span data-stu-id="9399a-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample1.aspx)]

<span data-ttu-id="9399a-114">Die Animation wird auf einen Bereich des Texts angewendet werden, der so aussieht:</span><span class="sxs-lookup"><span data-stu-id="9399a-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample2.aspx)]

<span data-ttu-id="9399a-115">Definieren Sie in der zugehörigen CSS-Klasse für den Bereich eine gute Hintergrundfarbe aus, und auch festlegen Sie eine feste Breite für den Bereich:</span><span class="sxs-lookup"><span data-stu-id="9399a-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](changing-an-animation-using-client-side-code-cs/samples/sample3.css)]

<span data-ttu-id="9399a-116">Die tatsächliche Animation wird durch eine HTML-Schaltfläche gestartet:</span><span class="sxs-lookup"><span data-stu-id="9399a-116">The actual animation is launched by an HTML button:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample4.aspx)]

<span data-ttu-id="9399a-117">Fügen Sie dann die `AnimationExtender` auf der Seite Bereitstellen einer `ID`, `TargetControlID` -Attribut und das obligatorische `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="9399a-117">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample5.aspx)]

<span data-ttu-id="9399a-118">Beachten Sie, dass es keine `<Animations>` Knoten innerhalb der `AnimationExtender` Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="9399a-118">Note that there is no `<Animations>` node within the `AnimationExtender` control.</span></span> <span data-ttu-id="9399a-119">Benutzerdefinierte JavaScript-Code wird verwendet, geben Sie die Animationen, die mit dem Steuerelement verwendet werden.</span><span class="sxs-lookup"><span data-stu-id="9399a-119">Custom JavaScript code is used to provide the animations to be used with the control.</span></span>

<span data-ttu-id="9399a-120">Wie bei der Server-API von `AnimationExtender`, es gibt keine einfache Möglichkeit, eine Animation für den Extender noch nicht zugewiesen.</span><span class="sxs-lookup"><span data-stu-id="9399a-120">As with the server API of `AnimationExtender`, there is no easy way to assign an animation to the extender yet.</span></span> <span data-ttu-id="9399a-121">Mit den verschiedenen Ereignissen registriert jedoch der Extender zur Verfügung stellt mehrere Methoden zum Lesen und Schreiben von Animationen (`OnClick`, `OnLoad`und so weiter).</span><span class="sxs-lookup"><span data-stu-id="9399a-121">However the extender does expose several methods to read and write animations registered with the various events (`OnClick`, `OnLoad`, and so on).</span></span> <span data-ttu-id="9399a-122">Hier einige Beispiele:</span><span class="sxs-lookup"><span data-stu-id="9399a-122">Here are some examples:</span></span>

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

<span data-ttu-id="9399a-123">Das Format des Rückgabewerts von der `get_*()` Funktionen und das Format des Arguments für die `set_*()` Funktionen ist eine JSON-Zeichenfolge, die eine objektdarstellung, der das XML-Markup bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="9399a-123">The format of the return value of the `get_*()` functions and the format of the argument for the `set_*()` functions is a JSON string, providing an object representation of what the XML markup would be.</span></span> <span data-ttu-id="9399a-124">Derzeit besteht keine Möglichkeit, ein Objekt übergeben, aber es ist möglich, ein Objekt aus einer angegebenen Animation lesen (`get_OnXXXBehavior()` Methoden).</span><span class="sxs-lookup"><span data-stu-id="9399a-124">Currently, there is no way to pass an object in, but it is possible to read an object from a given animation (`get_OnXXXBehavior()` methods).</span></span>

<span data-ttu-id="9399a-125">Hier ist eine JSON-Zeichenfolge (ohne die begrenzenden Anführungszeichen und ordentlich), die eine Animation, die von der Schaltfläche ausgelöste darstellt, aber die Animation des Bereichs durch Ändern der Größe und gleichzeitig ausblenden:</span><span class="sxs-lookup"><span data-stu-id="9399a-125">Here is a JSON string (without the delimiting quotes and formatted nicely) representing an animation triggered by the button, but animating the panel by resizing it and fading it out at the same time:</span></span>

[!code-json[Main](changing-an-animation-using-client-side-code-cs/samples/sample6.json)]

<span data-ttu-id="9399a-126">Die folgende JavaScript-Code weist diese JSON-descripting auf die `OnClick` Animation des aktuellen Extenders und führt ihn:</span><span class="sxs-lookup"><span data-stu-id="9399a-126">The following JavaScript code assigns this JSON descripting to the `OnClick` animation of the current extender and runs it:</span></span>

[!code-html[Main](changing-an-animation-using-client-side-code-cs/samples/sample7.html)]


<span data-ttu-id="9399a-127">[![Die Animation wird sofort ausgeführt, ohne einen Mausklick (und mit sehr geringem Markup)](changing-an-animation-using-client-side-code-cs/_static/image2.png)](changing-an-animation-using-client-side-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="9399a-127">[![The animation runs immediately, without a mouse click (and with very little markup)](changing-an-animation-using-client-side-code-cs/_static/image2.png)](changing-an-animation-using-client-side-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="9399a-128">Die Animation wird sofort ausgeführt, ohne ein Mausklick (und mit sehr geringem Markup) ([klicken Sie, um das Bild in voller Größe anzeigen](changing-an-animation-using-client-side-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="9399a-128">The animation runs immediately, without a mouse click (and with very little markup) ([Click to view full-size image](changing-an-animation-using-client-side-code-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9399a-129">[Zurück](executing-animations-using-client-side-code-cs.md)
> [Weiter](animating-an-updatepanel-control-cs.md)</span><span class="sxs-lookup"><span data-stu-id="9399a-129">[Previous](executing-animations-using-client-side-code-cs.md)
[Next](animating-an-updatepanel-control-cs.md)</span></span>
