---
uid: web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
title: "Ändern von Animationen von der Serverseite (VB) | Microsoft Docs"
author: wenz
description: "Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen. Es kann auch die Animationen an..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: addcf4aa-340a-460b-9c64-506424a1f725
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/modifying-animations-from-the-server-side-vb
msc.type: authoredcontent
ms.openlocfilehash: c5b23cce529be24157a8a3f9136de7ad7bafc1ea
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="modifying-animations-from-the-server-side-vb"></a><span data-ttu-id="41726-104">Ändern von Animationen von der Serverseite (VB)</span><span class="sxs-lookup"><span data-stu-id="41726-104">Modifying Animations From The Server Side (VB)</span></span>
====================
<span data-ttu-id="41726-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="41726-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="41726-106">[Herunterladen von Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.vb.zip) oder [PDF herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="41726-106">[Download Code](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation9.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation9VB.pdf)</span></span>

> <span data-ttu-id="41726-107">Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="41726-107">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="41726-108">Die Animationen möglicherweise auch auf der Serverseite geändert werden</span><span class="sxs-lookup"><span data-stu-id="41726-108">The animations may also be changed on the server-side</span></span>


## <a name="overview"></a><span data-ttu-id="41726-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="41726-109">Overview</span></span>

<span data-ttu-id="41726-110">Animation-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit ist nicht nur ein Steuerelement, aber eine gesamte Framework Animationen an ein Steuerelement hinzufügen.</span><span class="sxs-lookup"><span data-stu-id="41726-110">The Animation control in the ASP.NET AJAX Control Toolkit is not just a control but a whole framework to add animations to a control.</span></span> <span data-ttu-id="41726-111">Die Animationen möglicherweise auch auf der Serverseite geändert werden</span><span class="sxs-lookup"><span data-stu-id="41726-111">The animations may also be changed on the server-side</span></span>

## <a name="steps"></a><span data-ttu-id="41726-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="41726-112">Steps</span></span>

<span data-ttu-id="41726-113">Erstens sind die `ScriptManager` auf der Seite; klicken Sie dann die ASP.NET AJAX-Bibliothek geladen ist, wodurch das Steuerelement-Toolkit verwenden:</span><span class="sxs-lookup"><span data-stu-id="41726-113">First of all, include the `ScriptManager` in the page; then, the ASP.NET AJAX library is loaded, making it possible to use the Control Toolkit:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample1.aspx)]

<span data-ttu-id="41726-114">Die Animation wird auf einen Bereich des Texts angewendet werden, die wie folgt aussieht:</span><span class="sxs-lookup"><span data-stu-id="41726-114">The animation will be applied to a panel of text which looks like this:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample2.aspx)]

<span data-ttu-id="41726-115">Definieren Sie in der zugehörigen CSS-Klasse für den Bereich eine gute Hintergrundfarbe und auch festlegen Sie eine feste Breite für den Bereich:</span><span class="sxs-lookup"><span data-stu-id="41726-115">In the associated CSS class for the panel, define a nice background color and also set a fixed width for the panel:</span></span>

[!code-css[Main](modifying-animations-from-the-server-side-vb/samples/sample3.css)]

<span data-ttu-id="41726-116">Der Rest des Codes auf der Serverseite ausgeführt wird und Markup nicht verwendet. Stattdessen wird Code zum Erstellen der `AnimationExtender` Steuerelement:</span><span class="sxs-lookup"><span data-stu-id="41726-116">The rest of the code runs on the server-side and does not use markup; instead, it uses code to create the `AnimationExtender` control:</span></span>

[!code-aspx[Main](modifying-animations-from-the-server-side-vb/samples/sample4.aspx)]

<span data-ttu-id="41726-117">Das Steuerelement-Toolkit bietet derzeit jedoch kein API-Zugriff, um die einzelnen Animationen erstellen.</span><span class="sxs-lookup"><span data-stu-id="41726-117">However, the Control Toolkit currently does not provide an API access to create the individual animations.</span></span> <span data-ttu-id="41726-118">Es ist jedoch möglich ist, legen Sie die `AnimationExtender`die Animationen-Eigenschaft, um eine Zeichenfolge enthält, des XML-Markups verwendet, wenn die Animationen deklarativ zuweisen.</span><span class="sxs-lookup"><span data-stu-id="41726-118">It is however possible to set the `AnimationExtender`'s Animations property to a string containing the XML markup used when assigning the animations declaratively.</span></span> <span data-ttu-id="41726-119">Um den XML-Code erstellen, die nicht enthalten darf der `<Animations>` können Sie die .NET Framework-XML-Element unterstützen, oder geben Sie wie im folgenden Code nur die Zeichenfolge:</span><span class="sxs-lookup"><span data-stu-id="41726-119">In order to create the XML which must not contain the `<Animations>` element you could use the .NET Framework's XML support or, as in the following code, just provide the string:</span></span>

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample5.vb)]

<span data-ttu-id="41726-120">Fügen Sie schließlich die `AnimationExtender` innerhalb der aktuellen Seite steuern die `<form runat="server">` -Element, und stellen Sie sicher, dass die Animation enthalten ist und ausgeführt wird:</span><span class="sxs-lookup"><span data-stu-id="41726-120">Finally, add the `AnimationExtender` control to the current page, within the `<form runat="server">` element, making sure that the animation is included and runs:</span></span>

[!code-vb[Main](modifying-animations-from-the-server-side-vb/samples/sample6.vb)]


<span data-ttu-id="41726-121">[![Die Animation wird mit serverseitiger C#/VB-Code erstellt.](modifying-animations-from-the-server-side-vb/_static/image2.png)](modifying-animations-from-the-server-side-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="41726-121">[![The animation is created using server-side C#/VB code](modifying-animations-from-the-server-side-vb/_static/image2.png)](modifying-animations-from-the-server-side-vb/_static/image1.png)</span></span>

<span data-ttu-id="41726-122">Die Animation wird mithilfe von serverseitiger C#/VB-Code erstellt ([klicken Sie hier, um das Bild in voller Größe angezeigt](modifying-animations-from-the-server-side-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="41726-122">The animation is created using server-side C#/VB code ([Click to view full-size image](modifying-animations-from-the-server-side-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="41726-123">[Zurück](triggering-an-animation-in-another-control-vb.md)
[Weiter](executing-animations-using-client-side-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="41726-123">[Previous](triggering-an-animation-in-another-control-vb.md)
[Next](executing-animations-using-client-side-code-vb.md)</span></span>
