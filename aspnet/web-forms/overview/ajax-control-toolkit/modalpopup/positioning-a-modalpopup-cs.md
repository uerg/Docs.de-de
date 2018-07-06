---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-cs
title: Positionieren eines ModalPopup-Steuerelements (c#) | Microsoft-Dokumentation
author: wenz
description: Der ModalPopup-Steuerelement im AJAX Control Toolkit bietet eine einfache Möglichkeit, ein modales Fenster mithilfe der clientseitigen Methoden zu erstellen. Das Steuerelement jedoch keine bietet ein...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 1caac9d0-e21e-49d6-a8ff-e563a736d6ca
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: 4f94c0a473b88c0155847d700c48faa34b84c940
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807063"
---
<a name="positioning-a-modalpopup-c"></a><span data-ttu-id="d535c-104">Positionieren eines ModalPopup-Steuerelements (c#)</span><span class="sxs-lookup"><span data-stu-id="d535c-104">Positioning a ModalPopup (C#)</span></span>
====================
<span data-ttu-id="d535c-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="d535c-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="d535c-106">[Code herunterladen](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.cs.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="d535c-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4CS.pdf)</span></span>

> <span data-ttu-id="d535c-107">Der ModalPopup-Steuerelement im AJAX Control Toolkit bietet eine einfache Möglichkeit, ein modales Fenster mithilfe der clientseitigen Methoden zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="d535c-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="d535c-108">Das Steuerelement bietet jedoch keine integrierte Funktionen, die das Popup zu positionieren.</span><span class="sxs-lookup"><span data-stu-id="d535c-108">However the control does not offer a built-in functionality to position the popup.</span></span>


## <a name="overview"></a><span data-ttu-id="d535c-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="d535c-109">Overview</span></span>

<span data-ttu-id="d535c-110">Der ModalPopup-Steuerelement im AJAX Control Toolkit bietet eine einfache Möglichkeit, ein modales Fenster mithilfe der clientseitigen Methoden zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="d535c-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="d535c-111">Das Steuerelement bietet jedoch keine integrierte Funktionen, die das Popup zu positionieren.</span><span class="sxs-lookup"><span data-stu-id="d535c-111">However the control does not offer a built-in functionality to position the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="d535c-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="d535c-112">Steps</span></span>

<span data-ttu-id="d535c-113">Um die Funktionalität von ASP.NET AJAX und das Steuerelement-Toolkit, aktivieren die `ScriptManager`.</span><span class="sxs-lookup"><span data-stu-id="d535c-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager`.</span></span> <span data-ttu-id="d535c-114">Steuerelement an einer beliebigen Stelle auf der Seite platziert werden muss (jedoch innerhalb der `<form>` Element):</span><span class="sxs-lookup"><span data-stu-id="d535c-114">control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample1.aspx)]

<span data-ttu-id="d535c-115">Fügen Sie einen Bereich, der als modales Fenster dient.</span><span class="sxs-lookup"><span data-stu-id="d535c-115">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="d535c-116">Eine Schaltfläche wird verwendet, um das Popup zu schließen:</span><span class="sxs-lookup"><span data-stu-id="d535c-116">A button is used to close the popup:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample2.aspx)]

<span data-ttu-id="d535c-117">Wenn das Popup angezeigt wird, müssen sie an einer bestimmten Stelle auf der Seite angeordnet sein.</span><span class="sxs-lookup"><span data-stu-id="d535c-117">Whenever the popup is shown, it shall be positioned at a certain place in the page.</span></span> <span data-ttu-id="d535c-118">Für diese Aufgabe wird eine clientseitige JavaScript-Funktion erstellt.</span><span class="sxs-lookup"><span data-stu-id="d535c-118">For this task, a client-side JavaScript function is created.</span></span> <span data-ttu-id="d535c-119">Zuerst wird versucht, die dem Zugriffsbereich.</span><span class="sxs-lookup"><span data-stu-id="d535c-119">It first tries to access the panel.</span></span> <span data-ttu-id="d535c-120">Wenn dies gelingt, ist des Bereichs Position festgelegt mithilfe von CSS und JavaScript (Änderung, die die Position des Popupfensters am wird).</span><span class="sxs-lookup"><span data-stu-id="d535c-120">If it succeeds, the panel's position is set using CSS and JavaScript (change the position of the popup at will).</span></span> <span data-ttu-id="d535c-121">Jedoch `ModalPopupExtender` Steuerelement außerdem versucht wird, um das Popup zu positionieren.</span><span class="sxs-lookup"><span data-stu-id="d535c-121">However the `ModalPopupExtender` control also tries to position the popup.</span></span> <span data-ttu-id="d535c-122">Aus diesem Grund wird der JavaScript-Code Popupfenster jeder Zehntel Sekunde wiederholt positioniert.</span><span class="sxs-lookup"><span data-stu-id="d535c-122">Therefore, the JavaScript code repeatedly positions the popup, every tenth of a second.</span></span>

[!code-html[Main](positioning-a-modalpopup-cs/samples/sample3.html)]

<span data-ttu-id="d535c-123">Wie Sie sehen können, den Rückgabewert der `setTimeout()` JavaScript-Methode wird in einer globalen Variablen gespeichert.</span><span class="sxs-lookup"><span data-stu-id="d535c-123">As you can see, the return value of the `setTimeout()` JavaScript method is saved in a global variable.</span></span> <span data-ttu-id="d535c-124">Auf diese Weise beenden, die wiederholte Positionierung des Popups bei Bedarf kann mithilfe der `clearTimeout()` Methode:</span><span class="sxs-lookup"><span data-stu-id="d535c-124">This allows to stop the repeated positioning of the popup on demand, using the `clearTimeout()` method:</span></span>

[!code-javascript[Main](positioning-a-modalpopup-cs/samples/sample4.js)]

<span data-ttu-id="d535c-125">Nun müssen lediglich um den Browser, rufen diese Funktionen an, wo immer möglich zu machen.</span><span class="sxs-lookup"><span data-stu-id="d535c-125">Now all that is left to do is to make the browser call these functions whenever appropriate.</span></span> <span data-ttu-id="d535c-126">Die `movePanel()` JavaScript-Funktion muss aufgerufen werden, wenn die Schaltfläche geklickt wird, die den Bereich auslöst:</span><span class="sxs-lookup"><span data-stu-id="d535c-126">The `movePanel()` JavaScript function must be called when the button is clicked that triggers the panel:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample5.aspx)]

<span data-ttu-id="d535c-127">Und die `stopMoving()` Funktion ins Spiel, wenn das Popup kann geschlossen wird ausgelöst, der `ModalPopupExtender` Steuerelement:</span><span class="sxs-lookup"><span data-stu-id="d535c-127">And the `stopMoving()` function comes into play when the popup is closed this can be triggered in the `ModalPopupExtender` control:</span></span>

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample6.aspx)]


<span data-ttu-id="d535c-128">[![Die modales Fenster angezeigt wird, an der angegebenen position](positioning-a-modalpopup-cs/_static/image2.png)](positioning-a-modalpopup-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d535c-128">[![The modal popup appears at the designated position](positioning-a-modalpopup-cs/_static/image2.png)](positioning-a-modalpopup-cs/_static/image1.png)</span></span>

<span data-ttu-id="d535c-129">Die modales Fenster angezeigt wird, an der angegebenen Position ([klicken Sie, um das Bild in voller Größe anzeigen](positioning-a-modalpopup-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="d535c-129">The modal popup appears at the designated position ([Click to view full-size image](positioning-a-modalpopup-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d535c-130">[Zurück](handling-postbacks-from-a-modalpopup-cs.md)
> [Weiter](launching-a-modal-popup-window-from-server-code-vb.md)</span><span class="sxs-lookup"><span data-stu-id="d535c-130">[Previous](handling-postbacks-from-a-modalpopup-cs.md)
[Next](launching-a-modal-popup-window-from-server-code-vb.md)</span></span>
