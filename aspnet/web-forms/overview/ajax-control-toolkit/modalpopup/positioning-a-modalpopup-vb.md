---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
title: Positionieren ein ModalPopup (VB) | Microsoft Docs
author: wenz
description: ModalPopup-Steuerelement in das AJAX-Steuerelement-Toolkit bietet eine einfache Möglichkeit, ein modales Popupdialogfeld clientseitige Weise zu erstellen. Das Steuerelement jedoch keine bietet ein...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 8a07210c-eb0e-485e-9ee8-82a101520e65
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: 2d20888674dfedee7a7af85efd8df118c8394c6c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30874615"
---
<a name="positioning-a-modalpopup-vb"></a><span data-ttu-id="ceb4d-104">Positionieren ein ModalPopup (VB)</span><span class="sxs-lookup"><span data-stu-id="ceb4d-104">Positioning a ModalPopup (VB)</span></span>
====================
<span data-ttu-id="ceb4d-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="ceb4d-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="ceb4d-106">[Herunterladen von Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.vb.zip) oder [PDF herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="ceb4d-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4VB.pdf)</span></span>

> <span data-ttu-id="ceb4d-107">ModalPopup-Steuerelement in das AJAX-Steuerelement-Toolkit bietet eine einfache Möglichkeit, ein modales Popupdialogfeld clientseitige Weise zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="ceb4d-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="ceb4d-108">Das Steuerelement bietet jedoch nicht über eine integrierte Funktionalität, um das Popup zu positionieren.</span><span class="sxs-lookup"><span data-stu-id="ceb4d-108">However the control does not offer a built-in functionality to position the popup.</span></span>


## <a name="overview"></a><span data-ttu-id="ceb4d-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="ceb4d-109">Overview</span></span>

<span data-ttu-id="ceb4d-110">ModalPopup-Steuerelement in das AJAX-Steuerelement-Toolkit bietet eine einfache Möglichkeit, ein modales Popupdialogfeld clientseitige Weise zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="ceb4d-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="ceb4d-111">Das Steuerelement bietet jedoch nicht über eine integrierte Funktionalität, um das Popup zu positionieren.</span><span class="sxs-lookup"><span data-stu-id="ceb4d-111">However the control does not offer a built-in functionality to position the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="ceb4d-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="ceb4d-112">Steps</span></span>

<span data-ttu-id="ceb4d-113">Um die Funktionalität von ASP.NET AJAX und das Steuerelement-Toolkit aktivieren die `ScriptManager`.</span><span class="sxs-lookup"><span data-stu-id="ceb4d-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager`.</span></span> <span data-ttu-id="ceb4d-114">Steuerelement an einer beliebigen Stelle auf der Seite versetzt werden muss (jedoch innerhalb der `<form>` Element):</span><span class="sxs-lookup"><span data-stu-id="ceb4d-114">control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample1.aspx)]

<span data-ttu-id="ceb4d-115">Fügen Sie ein Panel die als modales Popupdialogfeld dient.</span><span class="sxs-lookup"><span data-stu-id="ceb4d-115">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="ceb4d-116">Eine Schaltfläche wird verwendet, um das Popup zu schließen:</span><span class="sxs-lookup"><span data-stu-id="ceb4d-116">A button is used to close the popup:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample2.aspx)]

<span data-ttu-id="ceb4d-117">Wenn das Popupfenster angezeigt wird, muss er an einem bestimmten Ort auf der Seite positioniert werden.</span><span class="sxs-lookup"><span data-stu-id="ceb4d-117">Whenever the popup is shown, it shall be positioned at a certain place in the page.</span></span> <span data-ttu-id="ceb4d-118">Für diese Aufgabe wird eine clientseitige JavaScript-Funktion erstellt.</span><span class="sxs-lookup"><span data-stu-id="ceb4d-118">For this task, a client-side JavaScript function is created.</span></span> <span data-ttu-id="ceb4d-119">Zuerst wird versucht, den Bereich zu gelangen.</span><span class="sxs-lookup"><span data-stu-id="ceb4d-119">It first tries to access the panel.</span></span> <span data-ttu-id="ceb4d-120">Wenn er erfolgreich ausgeführt wird, wird im Bereich positioniert mit CSS und JavaScript (ändern sich die Position des Popups an).</span><span class="sxs-lookup"><span data-stu-id="ceb4d-120">If it succeeds, the panel's position is set using CSS and JavaScript (change the position of the popup at will).</span></span> <span data-ttu-id="ceb4d-121">Jedoch `ModalPopupExtender` Steuerelement auch versucht wird, um das Popup zu positionieren.</span><span class="sxs-lookup"><span data-stu-id="ceb4d-121">However the `ModalPopupExtender` control also tries to position the popup.</span></span> <span data-ttu-id="ceb4d-122">Aus diesem Grund positioniert der JavaScript-Code wiederholt das Popupfenster alle Zehntelsekunde.</span><span class="sxs-lookup"><span data-stu-id="ceb4d-122">Therefore, the JavaScript code repeatedly positions the popup, every tenth of a second.</span></span>

[!code-html[Main](positioning-a-modalpopup-vb/samples/sample3.html)]

<span data-ttu-id="ceb4d-123">Wie können Sie sehen, den Rückgabewert der `setTimeout()` JavaScript-Methode in einer globalen Variablen gespeichert ist.</span><span class="sxs-lookup"><span data-stu-id="ceb4d-123">As you can see, the return value of the `setTimeout()` JavaScript method is saved in a global variable.</span></span> <span data-ttu-id="ceb4d-124">Auf diese Weise können die wiederholte Positionierung des Popups bei Bedarf beenden mithilfe der `clearTimeout()` Methode:</span><span class="sxs-lookup"><span data-stu-id="ceb4d-124">This allows to stop the repeated positioning of the popup on demand, using the `clearTimeout()` method:</span></span>

[!code-javascript[Main](positioning-a-modalpopup-vb/samples/sample4.js)]

<span data-ttu-id="ceb4d-125">Jetzt noch hierzu einen besteht darin, den Browser, die diese Funktionen jederzeit aufrufen.</span><span class="sxs-lookup"><span data-stu-id="ceb4d-125">Now all that is left to do is to make the browser call these functions whenever appropriate.</span></span> <span data-ttu-id="ceb4d-126">Die `movePanel()` JavaScript-Funktion muss aufgerufen werden, wenn die Schaltfläche geklickt wird, die im Bereich auslöst:</span><span class="sxs-lookup"><span data-stu-id="ceb4d-126">The `movePanel()` JavaScript function must be called when the button is clicked that triggers the panel:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample5.aspx)]

<span data-ttu-id="ceb4d-127">Und die `stopMoving()` Funktion kommt es zu einem geschlossenem das Popup Dies kann ausgelöst werden, der `ModalPopupExtender` Steuerelement:</span><span class="sxs-lookup"><span data-stu-id="ceb4d-127">And the `stopMoving()` function comes into play when the popup is closed this can be triggered in the `ModalPopupExtender` control:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-vb/samples/sample6.aspx)]


<span data-ttu-id="ceb4d-128">[![Das modale Popupfenster angezeigt wird, an der angegebenen position](positioning-a-modalpopup-vb/_static/image2.png)](positioning-a-modalpopup-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ceb4d-128">[![The modal popup appears at the designated position](positioning-a-modalpopup-vb/_static/image2.png)](positioning-a-modalpopup-vb/_static/image1.png)</span></span>

<span data-ttu-id="ceb4d-129">Das modale Popupfenster angezeigt wird, an der angegebenen Position ([klicken Sie hier, um das Bild in voller Größe angezeigt](positioning-a-modalpopup-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="ceb4d-129">The modal popup appears at the designated position ([Click to view full-size image](positioning-a-modalpopup-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ceb4d-130">Vorherige</span><span class="sxs-lookup"><span data-stu-id="ceb4d-130">Previous</span></span>](handling-postbacks-from-a-modalpopup-vb.md)
