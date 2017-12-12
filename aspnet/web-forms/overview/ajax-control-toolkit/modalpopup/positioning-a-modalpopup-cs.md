---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-cs
title: Positionieren ein ModalPopup (c#) | Microsoft Docs
author: wenz
description: "ModalPopup-Steuerelement in das AJAX-Steuerelement-Toolkit bietet eine einfache Möglichkeit, ein modales Popupdialogfeld clientseitige Weise zu erstellen. Das Steuerelement jedoch keine bietet ein..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 1caac9d0-e21e-49d6-a8ff-e563a736d6ca
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: 8dcc4e20ac98cbbad1ea3e86b7f895d32c853d4a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="positioning-a-modalpopup-c"></a><span data-ttu-id="2b230-104">Positionieren ein ModalPopup (c#)</span><span class="sxs-lookup"><span data-stu-id="2b230-104">Positioning a ModalPopup (C#)</span></span>
====================
<span data-ttu-id="2b230-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="2b230-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="2b230-106">[Herunterladen von Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.cs.zip) oder [PDF herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="2b230-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4CS.pdf)</span></span>

> <span data-ttu-id="2b230-107">ModalPopup-Steuerelement in das AJAX-Steuerelement-Toolkit bietet eine einfache Möglichkeit, ein modales Popupdialogfeld clientseitige Weise zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="2b230-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="2b230-108">Das Steuerelement bietet jedoch nicht über eine integrierte Funktionalität, um das Popup zu positionieren.</span><span class="sxs-lookup"><span data-stu-id="2b230-108">However the control does not offer a built-in functionality to position the popup.</span></span>


## <a name="overview"></a><span data-ttu-id="2b230-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="2b230-109">Overview</span></span>

<span data-ttu-id="2b230-110">ModalPopup-Steuerelement in das AJAX-Steuerelement-Toolkit bietet eine einfache Möglichkeit, ein modales Popupdialogfeld clientseitige Weise zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="2b230-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="2b230-111">Das Steuerelement bietet jedoch nicht über eine integrierte Funktionalität, um das Popup zu positionieren.</span><span class="sxs-lookup"><span data-stu-id="2b230-111">However the control does not offer a built-in functionality to position the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="2b230-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="2b230-112">Steps</span></span>

<span data-ttu-id="2b230-113">Um die Funktionalität von ASP.NET AJAX und das Steuerelement-Toolkit aktivieren die `ScriptManager`.</span><span class="sxs-lookup"><span data-stu-id="2b230-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager`.</span></span> <span data-ttu-id="2b230-114">Steuerelement an einer beliebigen Stelle auf der Seite versetzt werden muss (jedoch innerhalb der `<form>` Element):</span><span class="sxs-lookup"><span data-stu-id="2b230-114">control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample1.aspx)]

<span data-ttu-id="2b230-115">Fügen Sie ein Panel die als modales Popupdialogfeld dient.</span><span class="sxs-lookup"><span data-stu-id="2b230-115">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="2b230-116">Eine Schaltfläche wird verwendet, um das Popup zu schließen:</span><span class="sxs-lookup"><span data-stu-id="2b230-116">A button is used to close the popup:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample2.aspx)]

<span data-ttu-id="2b230-117">Wenn das Popupfenster angezeigt wird, muss er an einem bestimmten Ort auf der Seite positioniert werden.</span><span class="sxs-lookup"><span data-stu-id="2b230-117">Whenever the popup is shown, it shall be positioned at a certain place in the page.</span></span> <span data-ttu-id="2b230-118">Für diese Aufgabe wird eine clientseitige JavaScript-Funktion erstellt.</span><span class="sxs-lookup"><span data-stu-id="2b230-118">For this task, a client-side JavaScript function is created.</span></span> <span data-ttu-id="2b230-119">Zuerst wird versucht, den Bereich zu gelangen.</span><span class="sxs-lookup"><span data-stu-id="2b230-119">It first tries to access the panel.</span></span> <span data-ttu-id="2b230-120">Wenn er erfolgreich ausgeführt wird, wird im Bereich positioniert mit CSS und JavaScript (ändern sich die Position des Popups an).</span><span class="sxs-lookup"><span data-stu-id="2b230-120">If it succeeds, the panel's position is set using CSS and JavaScript (change the position of the popup at will).</span></span> <span data-ttu-id="2b230-121">Jedoch `ModalPopupExtender` Steuerelement auch versucht wird, um das Popup zu positionieren.</span><span class="sxs-lookup"><span data-stu-id="2b230-121">However the `ModalPopupExtender` control also tries to position the popup.</span></span> <span data-ttu-id="2b230-122">Aus diesem Grund positioniert der JavaScript-Code wiederholt das Popupfenster alle Zehntelsekunde.</span><span class="sxs-lookup"><span data-stu-id="2b230-122">Therefore, the JavaScript code repeatedly positions the popup, every tenth of a second.</span></span>

[!code-html[Main](positioning-a-modalpopup-cs/samples/sample3.html)]

<span data-ttu-id="2b230-123">Wie können Sie sehen, den Rückgabewert der `setTimeout()` JavaScript-Methode in einer globalen Variablen gespeichert ist.</span><span class="sxs-lookup"><span data-stu-id="2b230-123">As you can see, the return value of the `setTimeout()` JavaScript method is saved in a global variable.</span></span> <span data-ttu-id="2b230-124">Auf diese Weise können die wiederholte Positionierung des Popups bei Bedarf beenden mithilfe der `clearTimeout()` Methode:</span><span class="sxs-lookup"><span data-stu-id="2b230-124">This allows to stop the repeated positioning of the popup on demand, using the `clearTimeout()` method:</span></span>

[!code-javascript[Main](positioning-a-modalpopup-cs/samples/sample4.js)]

<span data-ttu-id="2b230-125">Jetzt noch hierzu einen besteht darin, den Browser, die diese Funktionen jederzeit aufrufen.</span><span class="sxs-lookup"><span data-stu-id="2b230-125">Now all that is left to do is to make the browser call these functions whenever appropriate.</span></span> <span data-ttu-id="2b230-126">Die `movePanel()` JavaScript-Funktion muss aufgerufen werden, wenn die Schaltfläche geklickt wird, die im Bereich auslöst:</span><span class="sxs-lookup"><span data-stu-id="2b230-126">The `movePanel()` JavaScript function must be called when the button is clicked that triggers the panel:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample5.aspx)]

<span data-ttu-id="2b230-127">Und die `stopMoving()` Funktion kommt es zu einem geschlossenem das Popup Dies kann ausgelöst werden, der `ModalPopupExtender` Steuerelement:</span><span class="sxs-lookup"><span data-stu-id="2b230-127">And the `stopMoving()` function comes into play when the popup is closed this can be triggered in the `ModalPopupExtender` control:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample6.aspx)]


<span data-ttu-id="2b230-128">[![Das modale Popupfenster angezeigt wird, an der angegebenen position](positioning-a-modalpopup-cs/_static/image2.png)](positioning-a-modalpopup-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2b230-128">[![The modal popup appears at the designated position](positioning-a-modalpopup-cs/_static/image2.png)](positioning-a-modalpopup-cs/_static/image1.png)</span></span>

<span data-ttu-id="2b230-129">Das modale Popupfenster angezeigt wird, an der angegebenen Position ([klicken Sie hier, um das Bild in voller Größe angezeigt](positioning-a-modalpopup-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="2b230-129">The modal popup appears at the designated position ([Click to view full-size image](positioning-a-modalpopup-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="2b230-130">[Zurück](handling-postbacks-from-a-modalpopup-cs.md)
[Weiter](launching-a-modal-popup-window-from-server-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="2b230-130">[Previous](handling-postbacks-from-a-modalpopup-cs.md)
[Next](launching-a-modal-popup-window-from-server-code-vb.md)</span></span>
