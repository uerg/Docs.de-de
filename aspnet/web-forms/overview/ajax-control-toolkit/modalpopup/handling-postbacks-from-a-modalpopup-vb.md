---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
title: Behandlung von Postbacks aus einem ModalPopup (VB) | Microsoft Docs
author: wenz
description: ModalPopup-Steuerelement in das AJAX-Steuerelement-Toolkit bietet eine einfache Möglichkeit, ein modales Popupdialogfeld clientseitige Weise zu erstellen. Muss bei einem pos darauf geachtet werden...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: f70ac2b3-900f-40fa-858f-ab057904506b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-vb
msc.type: authoredcontent
ms.openlocfilehash: 1aa2c42deb67015dd0b35edf4ba72d8d667ec88c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="handling-postbacks-from-a-modalpopup-vb"></a><span data-ttu-id="7ee09-104">Behandlung von Postbacks aus einem ModalPopup (VB)</span><span class="sxs-lookup"><span data-stu-id="7ee09-104">Handling Postbacks from a ModalPopup (VB)</span></span>
====================
<span data-ttu-id="7ee09-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="7ee09-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="7ee09-106">[Herunterladen von Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.vb.zip) oder [PDF herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="7ee09-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3VB.pdf)</span></span>

> <span data-ttu-id="7ee09-107">ModalPopup-Steuerelement in das AJAX-Steuerelement-Toolkit bietet eine einfache Möglichkeit, ein modales Popupdialogfeld clientseitige Weise zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="7ee09-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="7ee09-108">Muss darauf geachtet werden, wenn ein Postback aus innerhalb der Meldung erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="7ee09-108">Special care must be taken when a postback is created from within the popup.</span></span>


## <a name="overview"></a><span data-ttu-id="7ee09-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="7ee09-109">Overview</span></span>

<span data-ttu-id="7ee09-110">ModalPopup-Steuerelement in das AJAX-Steuerelement-Toolkit bietet eine einfache Möglichkeit, ein modales Popupdialogfeld clientseitige Weise zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="7ee09-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="7ee09-111">Muss darauf geachtet werden, wenn ein Postback aus innerhalb der Meldung erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="7ee09-111">Special care must be taken when a postback is created from within the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="7ee09-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="7ee09-112">Steps</span></span>

<span data-ttu-id="7ee09-113">Um die Funktionalität von ASP.NET AJAX und das Steuerelement-Toolkit aktivieren die `ScriptManager` Steuerelement an einer beliebigen Stelle auf der Seite versetzt werden muss (jedoch innerhalb der `<form>` Element):</span><span class="sxs-lookup"><span data-stu-id="7ee09-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample1.aspx)]

<span data-ttu-id="7ee09-114">Fügen Sie ein Panel die als modales Popupdialogfeld dient.</span><span class="sxs-lookup"><span data-stu-id="7ee09-114">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="7ee09-115">Vorhanden ist, kann der Benutzer einen Namen und eine e-Mail-Adresse eingeben.</span><span class="sxs-lookup"><span data-stu-id="7ee09-115">There, the user can enter a name and an email address.</span></span> <span data-ttu-id="7ee09-116">Eine Schaltfläche wird verwendet, um das Popup zu schließen und die Informationen speichern.</span><span class="sxs-lookup"><span data-stu-id="7ee09-116">A button is used to close the popup and save the information.</span></span> <span data-ttu-id="7ee09-117">Beachten Sie, dass die `OnClick` -Attribut festgelegt ist, sodass ein Postback tritt auf, wenn auf diese Schaltfläche geklickt wird:</span><span class="sxs-lookup"><span data-stu-id="7ee09-117">Note that the `OnClick` attribute is set so that a postback occurs when this button is clicked:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample2.aspx)]

<span data-ttu-id="7ee09-118">Die Seite selbst besteht aus zwei Bezeichnungen für genau die gleichen Informationen an: Name und e-Mail-Adresse.</span><span class="sxs-lookup"><span data-stu-id="7ee09-118">The page itself consists of two labels for exactly the same information: name and email address.</span></span> <span data-ttu-id="7ee09-119">Eine Schaltfläche wird verwendet, um die modales Popupdialogfeld auslösen:</span><span class="sxs-lookup"><span data-stu-id="7ee09-119">A button is used to trigger the modal popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample3.aspx)]

<span data-ttu-id="7ee09-120">Um das Popupfenster angezeigt zu machen, fügen die `ModalPopupExtender` Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="7ee09-120">In order to make the popup appear, add the `ModalPopupExtender` control.</span></span> <span data-ttu-id="7ee09-121">Legen Sie die `PopupControlID` -Attribut des Bereichs-ID und `TargetControlID` auf die Schaltfläche-ID:</span><span class="sxs-lookup"><span data-stu-id="7ee09-121">Set the `PopupControlID` attribute to the panel's ID and `TargetControlID` to the button's ID:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample4.aspx)]

<span data-ttu-id="7ee09-122">Jetzt immer die `Save` innerhalb der modales Popupdialogfeld geklickt wird, die serverseitige `SaveData()` Methode ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="7ee09-122">Now whenever the `Save` button within the modal popup is clicked, the server-side `SaveData()` method is executed.</span></span> <span data-ttu-id="7ee09-123">Vorhanden ist, konnten Sie die eingegebenen Daten in einem Datenspeicher gespeichert.</span><span class="sxs-lookup"><span data-stu-id="7ee09-123">There, you could save the entered data in a data store.</span></span> <span data-ttu-id="7ee09-124">Der Einfachheit halber nur die neuen Daten in der Bezeichnung ausgegeben:</span><span class="sxs-lookup"><span data-stu-id="7ee09-124">For the sake of simplicity, the new data is just output in the label:</span></span>

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample5.vb)]

<span data-ttu-id="7ee09-125">Darüber hinaus sollte das Textbox-Steuerelemente innerhalb der modales Popupdialogfeld mit dem aktuellen Namen und e-Mail-gefüllt werden.</span><span class="sxs-lookup"><span data-stu-id="7ee09-125">Also, the textbox controls within the modal popup should be filled with the current name and email.</span></span> <span data-ttu-id="7ee09-126">Dies ist jedoch nur erforderlich, wenn kein Postback auftritt.</span><span class="sxs-lookup"><span data-stu-id="7ee09-126">However this is only necessary when no postback occurs.</span></span> <span data-ttu-id="7ee09-127">Ist ein Postback, werden die ASP.NET-Funktion "ViewState" speichern die Textfelder ein, durch die entsprechenden Werte automatisch ausgefüllt.</span><span class="sxs-lookup"><span data-stu-id="7ee09-127">If there is a postback, the ASP.NET viewstate feature will automatically fill the textboxes with the appropriate values.</span></span>

[!code-vb[Main](handling-postbacks-from-a-modalpopup-vb/samples/sample6.vb)]


<span data-ttu-id="7ee09-128">[![Modale Popupdialogfeld verursacht einen Postback.](handling-postbacks-from-a-modalpopup-vb/_static/image2.png)](handling-postbacks-from-a-modalpopup-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="7ee09-128">[![The modal popup causes a postback](handling-postbacks-from-a-modalpopup-vb/_static/image2.png)](handling-postbacks-from-a-modalpopup-vb/_static/image1.png)</span></span>

<span data-ttu-id="7ee09-129">Modale Popupdialogfeld bewirkt, dass einen Postback ([klicken Sie hier, um das Bild in voller Größe angezeigt](handling-postbacks-from-a-modalpopup-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="7ee09-129">The modal popup causes a postback ([Click to view full-size image](handling-postbacks-from-a-modalpopup-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="7ee09-130">[Zurück](using-modalpopup-with-a-repeater-control-vb.md)
> [Weiter](positioning-a-modalpopup-vb.md)</span><span class="sxs-lookup"><span data-stu-id="7ee09-130">[Previous](using-modalpopup-with-a-repeater-control-vb.md)
[Next](positioning-a-modalpopup-vb.md)</span></span>
