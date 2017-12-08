---
uid: web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
title: "Ändern einer Animation mit einer clientseitigen Code (c#) | Microsoft Docs"
author: wenz
description: "Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. Es können auch die Animation..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 2bfbc5cc-f942-44b7-a62d-a29520f1da9a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/changing-an-animation-using-client-side-code-cs
msc.type: authoredcontent
ms.openlocfilehash: 6dcaeac073f54b0804fe3acf7ec22491b1cbbba5
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="changing-an-animation-using-client-side-code-c"></a><span data-ttu-id="1a113-104">Ändern einer Animation mit einer clientseitigen Code (c#)</span><span class="sxs-lookup"><span data-stu-id="1a113-104">Changing an Animation Using Client-Side Code (C#)</span></span>
====================
<span data-ttu-id="1a113-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="1a113-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="1a113-106">[Herunterladen von Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.cs.zip) oder [PDF herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="1a113-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation11.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation11CS.pdf)</span></span>

> <span data-ttu-id="1a113-107">Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="1a113-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="1a113-108">Die Animation kann auch mit benutzerdefinierten clientseitigen JavaScript-Code geändert werden.</span><span class="sxs-lookup"><span data-stu-id="1a113-108">The animation can also be changed using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="1a113-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="1a113-109">Overview</span></span>

<span data-ttu-id="1a113-110">Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="1a113-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="1a113-111">Die Animation kann auch mit benutzerdefinierten clientseitigen JavaScript-Code geändert werden.</span><span class="sxs-lookup"><span data-stu-id="1a113-111">The animation can also be changed using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="1a113-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="1a113-112">Steps</span></span>

<span data-ttu-id="1a113-113">Erstens sind die `ScriptManager` auf der Seite; klicken Sie dann die ASP.NET AJAX-Bibliothek geladen ist, wodurch das Steuerelement-Toolkit verwenden:</span><span class="sxs-lookup"><span data-stu-id="1a113-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample1.aspx)]

<span data-ttu-id="1a113-114">Die Animation wird auf einen Bereich des Texts angewendet werden, die wie folgt aussieht:</span><span class="sxs-lookup"><span data-stu-id="1a113-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample2.aspx)]

<span data-ttu-id="1a113-115">Definieren Sie in der zugehörigen CSS-Klasse für den Bereich eine gute Hintergrundfarbe und auch festlegen Sie eine feste Breite für den Bereich:</span><span class="sxs-lookup"><span data-stu-id="1a113-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](changing-an-animation-using-client-side-code-cs/samples/sample3.css)]

<span data-ttu-id="1a113-116">Die tatsächliche Animation wird durch eine HTML-Schaltfläche gestartet:</span><span class="sxs-lookup"><span data-stu-id="1a113-116">The actual animation is launched by an HTML button:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample4.aspx)]

<span data-ttu-id="1a113-117">Fügen Sie dann die `AnimationExtender` auf der Seite "Bereitstellen einer `ID`, die `TargetControlID` Attribut und der Auswahlparameter `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="1a113-117">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](changing-an-animation-using-client-side-code-cs/samples/sample5.aspx)]

<span data-ttu-id="1a113-118">Beachten Sie, dass es keine `<Animations>` Knoten innerhalb der `AnimationExtender` Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="1a113-118">Note that there is no `<Animations>` node within the `AnimationExtender` control.</span></span> <span data-ttu-id="1a113-119">Benutzerdefinierte JavaScript-Code dient zum Bereitstellen von Animationen mit dem Steuerelement verwendet werden soll.</span><span class="sxs-lookup"><span data-stu-id="1a113-119">Custom JavaScript code is used to provide the animations to be used with the control.</span></span>

<span data-ttu-id="1a113-120">Wie bei der Server-API von `AnimationExtender`, es ist keine einfache Möglichkeit, eine Animation der Extender noch zuweisen.</span><span class="sxs-lookup"><span data-stu-id="1a113-120">As with the server API of `AnimationExtender`, there is no easy way to assign an animation to the extender yet.</span></span> <span data-ttu-id="1a113-121">Mit den verschiedenen Ereignissen registriert jedoch die Extender zur Verfügung stellt mehrere Methoden zum Lesen und Schreiben von Animationen (`OnClick`, `OnLoad`usw.).</span><span class="sxs-lookup"><span data-stu-id="1a113-121">However the extender does expose several methods to read and write animations registered with the various events (`OnClick`, `OnLoad`, and so on).</span></span> <span data-ttu-id="1a113-122">Hier einige Beispiele:</span><span class="sxs-lookup"><span data-stu-id="1a113-122">Here are some examples:</span></span>

- `get_OnClick()`
- `set_OnClick()`
- `get_OnLoad()`
- `set_OnLoad()`
- `...`

<span data-ttu-id="1a113-123">Das Format des Rückgabewerts der der `get_*()` Funktionen und das Format des Arguments für die `set_*()` Funktionen ist eine JSON-Zeichenfolge, die eine objektdarstellung wäre dies das XML-Markup bereitstellen.</span><span class="sxs-lookup"><span data-stu-id="1a113-123">The format of the return value of the `get_*()` functions and the format of the argument for the `set_*()` functions is a JSON string, providing an object representation of what the XML markup would be.</span></span> <span data-ttu-id="1a113-124">Aktuell besteht keine Möglichkeit, um ein Objekt im, aber es ist möglich, ein Objekt aus einer angegebenen Animation lesen (`get_OnXXXBehavior()` Methoden).</span><span class="sxs-lookup"><span data-stu-id="1a113-124">Currently, there is no way to pass an object in, but it is possible to read an object from a given animation (`get_OnXXXBehavior()` methods).</span></span>

<span data-ttu-id="1a113-125">Hier ist eine JSON-Zeichenfolge (ohne die begrenzenden Anführungszeichen und ordentlich formatierte), die eine Animation, die ausgelöst wird, indem Sie die Schaltfläche darstellt, jedoch im Bereich durch Ändern der Größe und gleichzeitig ausblenden animieren:</span><span class="sxs-lookup"><span data-stu-id="1a113-125">Here is a JSON string (without the delimiting quotes and formatted nicely) representing an animation triggered by the button, but animating the panel by resizing it and fading it out at the same time:</span></span>

[!code-json[Main](changing-an-animation-using-client-side-code-cs/samples/sample6.json)]

<span data-ttu-id="1a113-126">Der folgende JavaScript-Code weist diese JSON descripting auf die `OnClick` Animation des aktuellen Extenders und führt sie:</span><span class="sxs-lookup"><span data-stu-id="1a113-126">The following JavaScript code assigns this JSON descripting to the `OnClick` animation of the current extender and runs it:</span></span>

[!code-html[Main](changing-an-animation-using-client-side-code-cs/samples/sample7.html)]


<span data-ttu-id="1a113-127">[![Die Animation wird sofort ausgeführt, ohne einen Mausklick (und mit wenig Markup)](changing-an-animation-using-client-side-code-cs/_static/image2.png)](changing-an-animation-using-client-side-code-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="1a113-127">[![The animation runs immediately, without a mouse click (and with very little markup)](changing-an-animation-using-client-side-code-cs/_static/image2.png)](changing-an-animation-using-client-side-code-cs/_static/image1.png)</span></span>

<span data-ttu-id="1a113-128">Die Animation wird sofort ausgeführt, ohne einen Mausklick (und mit wenig Markup) ([klicken Sie hier, um das Bild in voller Größe angezeigt](changing-an-animation-using-client-side-code-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="1a113-128">The animation runs immediately, without a mouse click (and with very little markup) ([Click to view full-size image](changing-an-animation-using-client-side-code-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="1a113-129">[Zurück](executing-animations-using-client-side-code-cs.md)
[Weiter](animating-an-updatepanel-control-cs.md)</span><span class="sxs-lookup"><span data-stu-id="1a113-129">[Previous](executing-animations-using-client-side-code-cs.md)
[Next](animating-an-updatepanel-control-cs.md)</span></span>
