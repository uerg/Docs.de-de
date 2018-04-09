---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
title: Starten ein modales Popupfenster von Servercode (VB) | Microsoft Docs
author: wenz
description: ModalPopup-Steuerelement in das AJAX-Steuerelement-Toolkit bietet eine einfache Möglichkeit, ein modales Popupdialogfeld clientseitige Weise zu erstellen. Einige Szenarien erfordern jedoch, t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 36ca81d7-906d-4db2-952b-add18a4ff421
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 46554dae60ad9cd13e97e5755e95cb2125d1fed9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="launching-a-modal-popup-window-from-server-code-vb"></a><span data-ttu-id="bb5c7-104">Starten ein modales Popupfenster von Servercode (VB)</span><span class="sxs-lookup"><span data-stu-id="bb5c7-104">Launching a Modal Popup Window from Server Code (VB)</span></span>
====================
<span data-ttu-id="bb5c7-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="bb5c7-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="bb5c7-106">[Herunterladen von Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.vb.zip) oder [PDF herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="bb5c7-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1VB.pdf)</span></span>

> <span data-ttu-id="bb5c7-107">ModalPopup-Steuerelement in das AJAX-Steuerelement-Toolkit bietet eine einfache Möglichkeit, ein modales Popupdialogfeld clientseitige Weise zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="bb5c7-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="bb5c7-108">Einige Szenarien erfordern jedoch, dass das Öffnen des modales Popupdialogfeld auf der Serverseite ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="bb5c7-108">However some scenarios require that the opening of the modal popup is triggered on the server-side.</span></span>


## <a name="overview"></a><span data-ttu-id="bb5c7-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="bb5c7-109">Overview</span></span>

<span data-ttu-id="bb5c7-110">ModalPopup-Steuerelement in das AJAX-Steuerelement-Toolkit bietet eine einfache Möglichkeit, ein modales Popupdialogfeld clientseitige Weise zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="bb5c7-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="bb5c7-111">Einige Szenarien erfordern jedoch, dass das Öffnen des modales Popupdialogfeld auf der Serverseite ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="bb5c7-111">However some scenarios require that the opening of the modal popup is triggered on the server-side.</span></span>

## <a name="steps"></a><span data-ttu-id="bb5c7-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="bb5c7-112">Steps</span></span>

<span data-ttu-id="bb5c7-113">Erstens muss ASP.NET Web Schaltflächensteuerelement veranschaulichen die Funktionsweise der ModalPopup-Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="bb5c7-113">First of all, an ASP.NET Button web control is required to demonstrate how the ModalPopup control works.</span></span> <span data-ttu-id="bb5c7-114">Fügen Sie eine solche Schaltfläche innerhalb der &lt;Formular&gt; Element auf einer neuen Seite:</span><span class="sxs-lookup"><span data-stu-id="bb5c7-114">Add such a button within the &lt;form&gt; element on a new page:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample1.aspx)]

<span data-ttu-id="bb5c7-115">Klicken Sie dann, benötigen Sie das Markup für das Popupfenster, die, das Sie erstellen möchten.</span><span class="sxs-lookup"><span data-stu-id="bb5c7-115">Then, you need the markup for the popup you want to create.</span></span> <span data-ttu-id="bb5c7-116">Definieren Sie ihn als eine `<asp:Panel>` steuern und stellen Sie sicher, dass sie ein Button-Steuerelement enthalten.</span><span class="sxs-lookup"><span data-stu-id="bb5c7-116">Define it as an `<asp:Panel>` control and make sure that it includes a Button control.</span></span> <span data-ttu-id="bb5c7-117">Das Steuerelement ModalPopup bietet die Funktionalität, um eine solche Schaltfläche Schließen das Popup zu machen; Andernfalls ist keine einfache Methode zum verschwinden zu lassen.</span><span class="sxs-lookup"><span data-stu-id="bb5c7-117">The ModalPopup control offers the functionality to make such a button close the popup; otherwise there is no easy way to let it vanish.</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample2.aspx)]

<span data-ttu-id="bb5c7-118">Fügen Sie als Nächstes das ModalPopup-Steuerelement aus dem ASP.NET AJAX-Toolkit auf der Seite.</span><span class="sxs-lookup"><span data-stu-id="bb5c7-118">Next add the ModalPopup control from the ASP.NET AJAX Toolkit to the page.</span></span> <span data-ttu-id="bb5c7-119">Legen Sie Eigenschaften für die Schaltfläche, die das Steuerelement lädt, die Schaltfläche "", wodurch das nicht mehr angezeigt, und die tatsächliche Popup-ID an.</span><span class="sxs-lookup"><span data-stu-id="bb5c7-119">Set properties for the button which loads the control, the button which makes it disappear, and the ID of the actual popup.</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample3.aspx)]

<span data-ttu-id="bb5c7-120">Wie bei allen Webseiten, die basierend auf ASP.NET AJAX; der Skript-Manager ist erforderlich, um die erforderlichen JavaScript-Bibliotheken für die anderen Ziel-Browser zu laden:</span><span class="sxs-lookup"><span data-stu-id="bb5c7-120">As with all web pages based on ASP.NET AJAX; the Script Manager is required to load the necessary JavaScript libraries for the different target browsers:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample4.aspx)]

<span data-ttu-id="bb5c7-121">Führen Sie das Beispiel im Browser.</span><span class="sxs-lookup"><span data-stu-id="bb5c7-121">Run the example in the browser.</span></span> <span data-ttu-id="bb5c7-122">Wenn Sie auf die Schaltfläche klicken, wird die modale Popupdialogfeld angezeigt.</span><span class="sxs-lookup"><span data-stu-id="bb5c7-122">When you click on the button, the modal popup appears.</span></span> <span data-ttu-id="bb5c7-123">Um den gleichen Effekt mit serverseitigen Code zu erreichen, ist eine Schaltfläche "Neu" erforderlich:</span><span class="sxs-lookup"><span data-stu-id="bb5c7-123">In order to achieve the same effect using server-side code, a new button is required:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample5.aspx)]

<span data-ttu-id="bb5c7-124">Wie Sie sehen können, wird mit einem Klick auf die Schaltfläche einen Postback generiert und führt die `ServerButton_Click()` Methode auf dem Server.</span><span class="sxs-lookup"><span data-stu-id="bb5c7-124">As you can see, a click on the button generates a postback and executes the `ServerButton_Click()` method on the server.</span></span> <span data-ttu-id="bb5c7-125">Bei dieser Methode wird eine JavaScript-Funktion wird aufgerufen, `launchModal()` wird ausgeführt, um genau zu sein, werden die JavaScript-Funktion ausgeführt, sobald die Seite geladen wurde:</span><span class="sxs-lookup"><span data-stu-id="bb5c7-125">In this method, a JavaScript function called `launchModal()` is executed to be exact, the JavaScript function will be executed once the page has been loaded:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample6.aspx)]

<span data-ttu-id="bb5c7-126">Die Aufgabe von `launchModal()` besteht darin, die ModalPopup anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="bb5c7-126">The job of `launchModal()` is to display the ModalPopup.</span></span> <span data-ttu-id="bb5c7-127">Die `launchModal()` Funktion ausgeführt wird, sobald die vollständige HTML-Seite geladen wurden.</span><span class="sxs-lookup"><span data-stu-id="bb5c7-127">The `launchModal()` function is executed once the complete HTML page has been loaded.</span></span> <span data-ttu-id="bb5c7-128">Zu diesem Zeitpunkt jedoch wurden das ASP.NET AJAX-Framework vollständig geladen noch nicht.</span><span class="sxs-lookup"><span data-stu-id="bb5c7-128">At that moment, however, the ASP.NET AJAX framework has not been fully loaded yet.</span></span> <span data-ttu-id="bb5c7-129">Aus diesem Grund die `launchModal()` Funktion wird nur eine Variable, die das Steuerelement ModalPopup später angezeigt werden muss:</span><span class="sxs-lookup"><span data-stu-id="bb5c7-129">Therefore, the `launchModal()` function just sets a variable that the ModalPopup control must be shown later on:</span></span>

[!code-html[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample7.html)]

<span data-ttu-id="bb5c7-130">Die `pageLoad()` JavaScript-Funktion ist eine spezielle Funktion, die ausgeführt wird, wenn ASP.NET AJAX vollständig geladen wurde.</span><span class="sxs-lookup"><span data-stu-id="bb5c7-130">The `pageLoad()` JavaScript function is a special function that gets executed once ASP.NET AJAX has been fully loaded.</span></span> <span data-ttu-id="bb5c7-131">Daher wir diese Funktion zum Anzeigen der ModalPopup-Steuerelement, jedoch nur, wenn Code hinzufügen `launchModal()` vor aufgerufen wurde:</span><span class="sxs-lookup"><span data-stu-id="bb5c7-131">Therefore we add code to this function to show the ModalPopup control, but only if `launchModal()` has been called before:</span></span>

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample8.js)]

<span data-ttu-id="bb5c7-132">Die `$find()` -Funktion sucht nach einem benannten Element auf der Seite und die serverseitige-ID als Parameter erwartet.</span><span class="sxs-lookup"><span data-stu-id="bb5c7-132">The `$find()` function is looking for a named element on the page and expects the server-side ID as a parameter.</span></span> <span data-ttu-id="bb5c7-133">Aus diesem Grund `$find("mpe")` gibt die Clientdarstellung des Steuerelements ModalPopup zurück, dessen `show()` Methode ermöglicht das Popupfenster angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="bb5c7-133">Therefore, `$find("mpe")` returns the client representation of the ModalPopup control; its `show()` method lets the popup appear.</span></span>


<span data-ttu-id="bb5c7-134">[![Modale Popupdialogfeld angezeigt, wenn eine der Schaltflächen geklickt wird](launching-a-modal-popup-window-from-server-code-vb/_static/image2.png)](launching-a-modal-popup-window-from-server-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="bb5c7-134">[![The modal popup appears when either of the buttons is clicked](launching-a-modal-popup-window-from-server-code-vb/_static/image2.png)](launching-a-modal-popup-window-from-server-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="bb5c7-135">Modale Popupdialogfeld angezeigt, wenn eine der Schaltflächen geklickt wird ([klicken Sie hier, um das Bild in voller Größe angezeigt](launching-a-modal-popup-window-from-server-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="bb5c7-135">The modal popup appears when either of the buttons is clicked ([Click to view full-size image](launching-a-modal-popup-window-from-server-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="bb5c7-136">[Zurück](positioning-a-modalpopup-cs.md)
> [Weiter](using-modalpopup-with-a-repeater-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="bb5c7-136">[Previous](positioning-a-modalpopup-cs.md)
[Next](using-modalpopup-with-a-repeater-control-vb.md)</span></span>
