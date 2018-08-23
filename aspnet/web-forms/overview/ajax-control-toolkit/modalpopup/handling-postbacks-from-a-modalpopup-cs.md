---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
title: Verarbeiten von Postbacks über ein ModalPopup-Steuerelement (c#) | Microsoft-Dokumentation
author: wenz
description: Der ModalPopup-Steuerelement im AJAX Control Toolkit bietet eine einfache Möglichkeit, ein modales Fenster mithilfe der clientseitigen Methoden zu erstellen. Besondere Sorgfalt muss ausgeführt werden, wenn ein pos...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 7963890b-4ea3-4a1c-b65d-6098a3d56f62
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: 9bcb1ad058b800f3f957cafff07a5a54b44178a6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823948"
---
<a name="handling-postbacks-from-a-modalpopup-c"></a><span data-ttu-id="6ca00-104">Verarbeiten von Postbacks über ein ModalPopup-Steuerelement (c#)</span><span class="sxs-lookup"><span data-stu-id="6ca00-104">Handling Postbacks from a ModalPopup (C#)</span></span>
====================
<span data-ttu-id="6ca00-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="6ca00-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="6ca00-106">[Code herunterladen](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.cs.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="6ca00-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.cs.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3CS.pdf)</span></span>

> <span data-ttu-id="6ca00-107">Der ModalPopup-Steuerelement im AJAX Control Toolkit bietet eine einfache Möglichkeit, ein modales Fenster mithilfe der clientseitigen Methoden zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="6ca00-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="6ca00-108">Besondere Sorgfalt muss ausgeführt werden, wenn ein Postback aus in das Popup erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="6ca00-108">Special care must be taken when a postback is created from within the popup.</span></span>


## <a name="overview"></a><span data-ttu-id="6ca00-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="6ca00-109">Overview</span></span>

<span data-ttu-id="6ca00-110">Der ModalPopup-Steuerelement im AJAX Control Toolkit bietet eine einfache Möglichkeit, ein modales Fenster mithilfe der clientseitigen Methoden zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="6ca00-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="6ca00-111">Besondere Sorgfalt muss ausgeführt werden, wenn ein Postback aus in das Popup erstellt wird.</span><span class="sxs-lookup"><span data-stu-id="6ca00-111">Special care must be taken when a postback is created from within the popup.</span></span>

## <a name="steps"></a><span data-ttu-id="6ca00-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="6ca00-112">Steps</span></span>

<span data-ttu-id="6ca00-113">Um die Funktionalität von ASP.NET AJAX und das Steuerelement-Toolkit, aktivieren die `ScriptManager` Steuerelement an einer beliebigen Stelle auf der Seite platziert werden muss (jedoch innerhalb der `<form>` Element):</span><span class="sxs-lookup"><span data-stu-id="6ca00-113">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample1.aspx)]

<span data-ttu-id="6ca00-114">Fügen Sie einen Bereich, der als modales Fenster dient.</span><span class="sxs-lookup"><span data-stu-id="6ca00-114">Next, add a panel which serves as the modal popup.</span></span> <span data-ttu-id="6ca00-115">Dort kann der Benutzer einen Namen und eine e-Mail-Adresse eingeben.</span><span class="sxs-lookup"><span data-stu-id="6ca00-115">There, the user can enter a name and an email address.</span></span> <span data-ttu-id="6ca00-116">Eine Schaltfläche wird verwendet, um das Popup zu schließen und speichern Sie die Informationen.</span><span class="sxs-lookup"><span data-stu-id="6ca00-116">A button is used to close the popup and save the information.</span></span> <span data-ttu-id="6ca00-117">Beachten Sie, dass die `OnClick` -Attribut festgelegt ist, sodass ein Postback auftritt, wenn auf diese Schaltfläche geklickt wird:</span><span class="sxs-lookup"><span data-stu-id="6ca00-117">Note that the `OnClick` attribute is set so that a postback occurs when this button is clicked:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample2.aspx)]

<span data-ttu-id="6ca00-118">Die Seite selbst besteht aus zwei Bezeichnungen für genau die gleichen Informationen: Name und e-Mail-Adresse.</span><span class="sxs-lookup"><span data-stu-id="6ca00-118">The page itself consists of two labels for exactly the same information: name and email address.</span></span> <span data-ttu-id="6ca00-119">Eine Schaltfläche dient zum modalen Popups auslösen:</span><span class="sxs-lookup"><span data-stu-id="6ca00-119">A button is used to trigger the modal popup:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample3.aspx)]

<span data-ttu-id="6ca00-120">Um das Popup angezeigt werden können, fügen die `ModalPopupExtender` Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="6ca00-120">In order to make the popup appear, add the `ModalPopupExtender` control.</span></span> <span data-ttu-id="6ca00-121">Legen Sie die `PopupControlID` Attribut des Bereichs-ID und `TargetControlID` auf die Schaltfläche "ID:</span><span class="sxs-lookup"><span data-stu-id="6ca00-121">Set the `PopupControlID` attribute to the panel's ID and `TargetControlID` to the button's ID:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample4.aspx)]

<span data-ttu-id="6ca00-122">Jetzt immer die `Save` innerhalb des modalen Popups geklickt wird, die serverseitige `SaveData()` Methode ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="6ca00-122">Now whenever the `Save` button within the modal popup is clicked, the server-side `SaveData()` method is executed.</span></span> <span data-ttu-id="6ca00-123">Dort können Sie die eingegebenen Daten in einem Datenspeicher sparen.</span><span class="sxs-lookup"><span data-stu-id="6ca00-123">There, you could save the entered data in a data store.</span></span> <span data-ttu-id="6ca00-124">Der Einfachheit halber werden die neuen Daten nur in der Bezeichnung Ausgabe:</span><span class="sxs-lookup"><span data-stu-id="6ca00-124">For the sake of simplicity, the new data is just output in the label:</span></span>

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample5.cs)]

<span data-ttu-id="6ca00-125">Darüber hinaus sollte die Textbox-Steuerelemente innerhalb des modalen Popups mit den aktuellen Namen und e-Mail-Adresse gefüllt werden.</span><span class="sxs-lookup"><span data-stu-id="6ca00-125">Also, the textbox controls within the modal popup should be filled with the current name and email.</span></span> <span data-ttu-id="6ca00-126">Dies ist jedoch nur erforderlich, wenn kein Postback auftritt.</span><span class="sxs-lookup"><span data-stu-id="6ca00-126">However this is only necessary when no postback occurs.</span></span> <span data-ttu-id="6ca00-127">Ist es ein Postback, füllt die Viewstate-Funktion von ASP.NET automatisch die Textfelder ein, durch die entsprechenden Werte.</span><span class="sxs-lookup"><span data-stu-id="6ca00-127">If there is a postback, the ASP.NET viewstate feature will automatically fill the textboxes with the appropriate values.</span></span>

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample6.cs)]


<span data-ttu-id="6ca00-128">[![Modale Fenster auslöst ein Postback.](handling-postbacks-from-a-modalpopup-cs/_static/image2.png)](handling-postbacks-from-a-modalpopup-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="6ca00-128">[![The modal popup causes a postback](handling-postbacks-from-a-modalpopup-cs/_static/image2.png)](handling-postbacks-from-a-modalpopup-cs/_static/image1.png)</span></span>

<span data-ttu-id="6ca00-129">Modale Fenster ein Postback auslöst ([klicken Sie, um das Bild in voller Größe anzeigen](handling-postbacks-from-a-modalpopup-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="6ca00-129">The modal popup causes a postback ([Click to view full-size image](handling-postbacks-from-a-modalpopup-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6ca00-130">[Zurück](using-modalpopup-with-a-repeater-control-cs.md)
> [Weiter](positioning-a-modalpopup-cs.md)</span><span class="sxs-lookup"><span data-stu-id="6ca00-130">[Previous](using-modalpopup-with-a-repeater-control-cs.md)
[Next](positioning-a-modalpopup-cs.md)</span></span>
