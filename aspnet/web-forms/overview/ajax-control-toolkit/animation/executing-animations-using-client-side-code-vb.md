---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
title: Ausführen von Animationen mit clientseitigem Code (VB) | Microsoft-Dokumentation
author: wenz
description: Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Die Ausführung der Animation...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: f7073f50-d765-456d-9957-926ce60f35f6
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-animations-using-client-side-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 9b180ad17e0e3d2dffa6262d10a83a8353e0a1d0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37811885"
---
<a name="executing-animations-using-client-side-code-vb"></a><span data-ttu-id="9c211-104">Ausführen von Animationen mit clientseitigem Code (VB)</span><span class="sxs-lookup"><span data-stu-id="9c211-104">Executing Animations Using Client-Side Code (VB)</span></span>
====================
<span data-ttu-id="9c211-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="9c211-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="9c211-106">[Code herunterladen](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.vb.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="9c211-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation10.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation10VB.pdf)</span></span>

> <span data-ttu-id="9c211-107">Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="9c211-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="9c211-108">Die Ausführung der Animation kann auch mit benutzerdefinierte clientseitige JavaScript-Code ausgelöst werden.</span><span class="sxs-lookup"><span data-stu-id="9c211-108">The animation execution may also be triggered using custom client-side JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="9c211-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="9c211-109">Overview</span></span>

<span data-ttu-id="9c211-110">Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="9c211-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="9c211-111">Die Ausführung der Animation kann auch mit benutzerdefinierte clientseitige JavaScript-Code ausgelöst werden.</span><span class="sxs-lookup"><span data-stu-id="9c211-111">The animation execution may also be triggered using custom client-side JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="9c211-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="9c211-112">Steps</span></span>

<span data-ttu-id="9c211-113">Zunächst einmal sind die `ScriptManager` in die Seite klicken Sie dann die ASP.NET AJAX-Bibliothek wird geladen, lässt sich das Steuerelement-Toolkit verwenden:</span><span class="sxs-lookup"><span data-stu-id="9c211-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample1.aspx)]

<span data-ttu-id="9c211-114">Die Animation wird auf einen Bereich des Texts angewendet werden, der so aussieht:</span><span class="sxs-lookup"><span data-stu-id="9c211-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample2.aspx)]

<span data-ttu-id="9c211-115">Definieren Sie in der zugehörigen CSS-Klasse für den Bereich eine gute Hintergrundfarbe aus, und auch festlegen Sie eine feste Breite für den Bereich:</span><span class="sxs-lookup"><span data-stu-id="9c211-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](executing-animations-using-client-side-code-vb/samples/sample3.css)]

<span data-ttu-id="9c211-116">Fügen Sie dann die `AnimationExtender` auf der Seite Bereitstellen einer `ID`, `TargetControlID` -Attribut und das obligatorische `runat="server"`:</span><span class="sxs-lookup"><span data-stu-id="9c211-116">Then, add the `AnimationExtender` to the page, providing an `ID`, the `TargetControlID` attribute and the obligatory `runat="server"`:</span></span>

[!code-aspx[Main](executing-animations-using-client-side-code-vb/samples/sample4.aspx)]

<span data-ttu-id="9c211-117">In der `<Animations>` Knoten verwenden `<OnClick>` klickt Sie auf den Animationen nach dem Benutzer ausgeführt werden im Bereich.</span><span class="sxs-lookup"><span data-stu-id="9c211-117">Within the `<Animations>` node, use `<OnClick>` to run the animations once the user clicks on the panel.</span></span> <span data-ttu-id="9c211-118">Fügen Sie zwei Animationen parallelly ausgeführt werden:</span><span class="sxs-lookup"><span data-stu-id="9c211-118">Add two animations to be executed parallelly:</span></span>

[!code-xml[Main](executing-animations-using-client-side-code-vb/samples/sample5.xml)]

<span data-ttu-id="9c211-119">Die Zwecke dieser Demo werden dieser Animation (und alle anderen Animationen erstellt, mit dem Steuerelement-Toolkit) ausgeführt mithilfe von JavaScript-Code, sobald die Seite ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="9c211-119">For the sake of demonstration, this animation (and any other animation created using the Control Toolkit) is executed using JavaScript code, once the page runs.</span></span> <span data-ttu-id="9c211-120">Zunächst einmal benötigen wir Zugriff auf die `AnimationExtender` Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="9c211-120">First of all we need access to the `AnimationExtender` control.</span></span> <span data-ttu-id="9c211-121">Die ASP.NET AJAX-Bibliothek stellt die `$find()` -Funktion für diese Aufgabe:</span><span class="sxs-lookup"><span data-stu-id="9c211-121">The ASP.NET AJAX library provides the `$find()` function for this task:</span></span>

[!code-csharp[Main](executing-animations-using-client-side-code-vb/samples/sample6.cs)]

<span data-ttu-id="9c211-122">Die `AnimationExtender` Steuerelement stellt eine umfangreiche API, einschließlich der Methoden mit Namen, die identisch mit die Ereignishandler, die in das XML-Markup verwendet: `OnClick()`, `OnLoad()`und so weiter.</span><span class="sxs-lookup"><span data-stu-id="9c211-122">The `AnimationExtender` control exposes a rich API, including methods with names identical to the event handlers used in the XML markup: `OnClick()`, `OnLoad()`, and so on.</span></span> <span data-ttu-id="9c211-123">Z. B. einen Aufruf von der `OnClick()` Methode führt die Animation innerhalb der `<OnClick>` Element der `AnimationExtender` Steuerelement:</span><span class="sxs-lookup"><span data-stu-id="9c211-123">For instance, a call of the `OnClick()` method executes the animation within the `<OnClick>` element of the `AnimationExtender` control:</span></span>

[!code-javascript[Main](executing-animations-using-client-side-code-vb/samples/sample7.js)]

<span data-ttu-id="9c211-124">Hier ist der vollständige clientseitige JavaScript-Code, der dem Klicken auf den Bereich emuliert, sobald die Seite vollständig geladen wurde Beachten Sie, dass die `pageLoad()` Funktionsnamen wird verwendet, die von ASP.NET AJAX nach der Seite aufgerufen wird und alle enthaltenen JavaScript-Bibliotheken wurden geladen.</span><span class="sxs-lookup"><span data-stu-id="9c211-124">Here is the complete client-side JavaScript code that emulates the click on the panel once the page has been fully loaded note that the `pageLoad()` function name is used which is called by ASP.NET AJAX once the page and all included JavaScript libraries have been loaded.</span></span>

[!code-html[Main](executing-animations-using-client-side-code-vb/samples/sample8.html)]


<span data-ttu-id="9c211-125">[![Die Animation wird sofort ohne ein Mausklick ausgeführt.](executing-animations-using-client-side-code-vb/_static/image2.png)](executing-animations-using-client-side-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="9c211-125">[![The animation runs immediately, without a mouse click](executing-animations-using-client-side-code-vb/_static/image2.png)](executing-animations-using-client-side-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="9c211-126">Die Animation wird sofort ausgeführt, ohne ein Mausklick ([klicken Sie, um das Bild in voller Größe anzeigen](executing-animations-using-client-side-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="9c211-126">The animation runs immediately, without a mouse click ([Click to view full-size image](executing-animations-using-client-side-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="9c211-127">[Zurück](modifying-animations-from-the-server-side-vb.md)
> [Weiter](changing-an-animation-using-client-side-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="9c211-127">[Previous](modifying-animations-from-the-server-side-vb.md)
[Next](changing-an-animation-using-client-side-code-vb.md)</span></span>
