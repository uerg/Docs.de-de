---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
title: Starten eines modalen Popupfensters über den Servercode (VB) | Microsoft-Dokumentation
author: wenz
description: Der ModalPopup-Steuerelement im AJAX Control Toolkit bietet eine einfache Möglichkeit, ein modales Fenster mithilfe der clientseitigen Methoden zu erstellen. Einige Szenarien erfordern jedoch, t...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 36ca81d7-906d-4db2-952b-add18a4ff421
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 52eec262a9241aa7b11cbb892bdf2142c7a2b83a
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37822767"
---
<a name="launching-a-modal-popup-window-from-server-code-vb"></a><span data-ttu-id="06885-104">Starten eines modalen Popupfensters über den Servercode (VB)</span><span class="sxs-lookup"><span data-stu-id="06885-104">Launching a Modal Popup Window from Server Code (VB)</span></span>
====================
<span data-ttu-id="06885-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="06885-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="06885-106">[Code herunterladen](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.vb.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="06885-106">[Download Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1VB.pdf)</span></span>

> <span data-ttu-id="06885-107">Der ModalPopup-Steuerelement im AJAX Control Toolkit bietet eine einfache Möglichkeit, ein modales Fenster mithilfe der clientseitigen Methoden zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="06885-107">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="06885-108">Einige Szenarien erfordern jedoch, dass das Öffnen eines modalen Popups auf der Serverseite ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="06885-108">However some scenarios require that the opening of the modal popup is triggered on the server-side.</span></span>


## <a name="overview"></a><span data-ttu-id="06885-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="06885-109">Overview</span></span>

<span data-ttu-id="06885-110">Der ModalPopup-Steuerelement im AJAX Control Toolkit bietet eine einfache Möglichkeit, ein modales Fenster mithilfe der clientseitigen Methoden zu erstellen.</span><span class="sxs-lookup"><span data-stu-id="06885-110">The ModalPopup control in the AJAX Control Toolkit offers a simple way to create a modal popup using client-side means.</span></span> <span data-ttu-id="06885-111">Einige Szenarien erfordern jedoch, dass das Öffnen eines modalen Popups auf der Serverseite ausgelöst wird.</span><span class="sxs-lookup"><span data-stu-id="06885-111">However some scenarios require that the opening of the modal popup is triggered on the server-side.</span></span>

## <a name="steps"></a><span data-ttu-id="06885-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="06885-112">Steps</span></span>

<span data-ttu-id="06885-113">Zunächst muss eine ASP.NET-Schaltfläche-Websteuerelement zur Veranschaulichung der Funktionsweise des ModalPopup-Steuerelements.</span><span class="sxs-lookup"><span data-stu-id="06885-113">First of all, an ASP.NET Button web control is required to demonstrate how the ModalPopup control works.</span></span> <span data-ttu-id="06885-114">Fügen Sie eine solche Schaltfläche innerhalb der &lt;Formular&gt; Element auf einer neuen Seite:</span><span class="sxs-lookup"><span data-stu-id="06885-114">Add such a button within the &lt;form&gt; element on a new page:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample1.aspx)]

<span data-ttu-id="06885-115">Klicken Sie dann benötigen Sie das Markup für das Popup, die, das Sie erstellen möchten.</span><span class="sxs-lookup"><span data-stu-id="06885-115">Then, you need the markup for the popup you want to create.</span></span> <span data-ttu-id="06885-116">Definieren ihn als einen `<asp:Panel>` steuern, und stellen Sie sicher, dass sie ein Schaltflächen-Steuerelement enthält.</span><span class="sxs-lookup"><span data-stu-id="06885-116">Define it as an `<asp:Panel>` control and make sure that it includes a Button control.</span></span> <span data-ttu-id="06885-117">Der ModalPopup-Steuerelement bietet die Funktionalität, um eine solche Schaltfläche Schließen des Popups stellen; Andernfalls ist es keine einfache Möglichkeit, die sie verschwinden zu lassen.</span><span class="sxs-lookup"><span data-stu-id="06885-117">The ModalPopup control offers the functionality to make such a button close the popup; otherwise there is no easy way to let it vanish.</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample2.aspx)]

<span data-ttu-id="06885-118">Als Nächstes können Sie dem ModalPopup-Steuerelement aus dem ASP.NET AJAX-Toolkit auf der Seite hinzu.</span><span class="sxs-lookup"><span data-stu-id="06885-118">Next add the ModalPopup control from the ASP.NET AJAX Toolkit to the page.</span></span> <span data-ttu-id="06885-119">Legen Sie Eigenschaften für die Schaltfläche, die das Steuerelement geladen, die Taste, die sie nicht mehr angezeigt wird und die ID des tatsächlichen Popups.</span><span class="sxs-lookup"><span data-stu-id="06885-119">Set properties for the button which loads the control, the button which makes it disappear, and the ID of the actual popup.</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample3.aspx)]

<span data-ttu-id="06885-120">Wie bei allen Webseiten, die basierend auf ASP.NET AJAX; der Skript-Manager erforderlich, um die erforderlichen JavaScript-Bibliotheken für die anderen Browser zu laden:</span><span class="sxs-lookup"><span data-stu-id="06885-120">As with all web pages based on ASP.NET AJAX; the Script Manager is required to load the necessary JavaScript libraries for the different target browsers:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample4.aspx)]

<span data-ttu-id="06885-121">Führen Sie das Beispiel im Browser.</span><span class="sxs-lookup"><span data-stu-id="06885-121">Run the example in the browser.</span></span> <span data-ttu-id="06885-122">Wenn Sie auf die Schaltfläche klicken, wird das modale Popup angezeigt.</span><span class="sxs-lookup"><span data-stu-id="06885-122">When you click on the button, the modal popup appears.</span></span> <span data-ttu-id="06885-123">Um den gleichen Effekt mit serverseitigem Code zu erreichen, ist eine neue Schaltfläche erforderlich:</span><span class="sxs-lookup"><span data-stu-id="06885-123">In order to achieve the same effect using server-side code, a new button is required:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample5.aspx)]

<span data-ttu-id="06885-124">Wie Sie sehen können, ein Klick auf die Schaltfläche mit den generiert ein Postback und führt die `ServerButton_Click()` Methode auf dem Server.</span><span class="sxs-lookup"><span data-stu-id="06885-124">As you can see, a click on the button generates a postback and executes the `ServerButton_Click()` method on the server.</span></span> <span data-ttu-id="06885-125">Bei dieser Methode eine JavaScript-Funktion wird aufgerufen, `launchModal()` wird ausgeführt, um genau zu sein, die JavaScript-Funktion ausgeführt wird, sobald die Seite geladen wurde:</span><span class="sxs-lookup"><span data-stu-id="06885-125">In this method, a JavaScript function called `launchModal()` is executed to be exact, the JavaScript function will be executed once the page has been loaded:</span></span>

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample6.aspx)]

<span data-ttu-id="06885-126">Die Aufgabe des `launchModal()` besteht darin, den ModalPopup-Steuerelements anzuzeigen.</span><span class="sxs-lookup"><span data-stu-id="06885-126">The job of `launchModal()` is to display the ModalPopup.</span></span> <span data-ttu-id="06885-127">Die `launchModal()` Funktion ausgeführt wird, nachdem die vollständige HTML-Seite geladen wurde.</span><span class="sxs-lookup"><span data-stu-id="06885-127">The `launchModal()` function is executed once the complete HTML page has been loaded.</span></span> <span data-ttu-id="06885-128">In diesem Moment aber wurde ASP.NET AJAX-Framework vollständig geladen noch nicht.</span><span class="sxs-lookup"><span data-stu-id="06885-128">At that moment, however, the ASP.NET AJAX framework has not been fully loaded yet.</span></span> <span data-ttu-id="06885-129">Aus diesem Grund die `launchModal()` Funktion wird nur eine Variable, die das ModalPopup-Steuerelement muss einem späteren Zeitpunkt angezeigt werden:</span><span class="sxs-lookup"><span data-stu-id="06885-129">Therefore, the `launchModal()` function just sets a variable that the ModalPopup control must be shown later on:</span></span>

[!code-html[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample7.html)]

<span data-ttu-id="06885-130">Die `pageLoad()` JavaScript-Funktion ist eine spezielle Sigmoidfunktion, die ausgeführt wird, nachdem ASP.NET AJAX vollständig geladen wurde.</span><span class="sxs-lookup"><span data-stu-id="06885-130">The `pageLoad()` JavaScript function is a special function that gets executed once ASP.NET AJAX has been fully loaded.</span></span> <span data-ttu-id="06885-131">Aus diesem Grund fügen wir Code für diese Funktion zum Anzeigen der ModalPopup-Steuerelement, aber nur, wenn `launchModal()` vor aufgerufen wurde:</span><span class="sxs-lookup"><span data-stu-id="06885-131">Therefore we add code to this function to show the ModalPopup control, but only if `launchModal()` has been called before:</span></span>

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample8.js)]

<span data-ttu-id="06885-132">Die `$find()` -Funktion sucht nach einem benannten Element auf der Seite und die serverseitige-ID als Parameter erwartet.</span><span class="sxs-lookup"><span data-stu-id="06885-132">The `$find()` function is looking for a named element on the page and expects the server-side ID as a parameter.</span></span> <span data-ttu-id="06885-133">Aus diesem Grund `$find("mpe")` gibt die Clientdarstellung des Steuerelements "ModalPopup" zurück, dessen `show()` -Methode können Sie das Popup angezeigt werden.</span><span class="sxs-lookup"><span data-stu-id="06885-133">Therefore, `$find("mpe")` returns the client representation of the ModalPopup control; its `show()` method lets the popup appear.</span></span>


<span data-ttu-id="06885-134">[![Modale Fenster wird angezeigt, wenn einer der Schaltflächen geklickt wird](launching-a-modal-popup-window-from-server-code-vb/_static/image2.png)](launching-a-modal-popup-window-from-server-code-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="06885-134">[![The modal popup appears when either of the buttons is clicked](launching-a-modal-popup-window-from-server-code-vb/_static/image2.png)](launching-a-modal-popup-window-from-server-code-vb/_static/image1.png)</span></span>

<span data-ttu-id="06885-135">Modale Fenster wird angezeigt, wenn einer der Schaltflächen geklickt wird ([klicken Sie, um das Bild in voller Größe anzeigen](launching-a-modal-popup-window-from-server-code-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="06885-135">The modal popup appears when either of the buttons is clicked ([Click to view full-size image](launching-a-modal-popup-window-from-server-code-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="06885-136">[Zurück](positioning-a-modalpopup-cs.md)
> [Weiter](using-modalpopup-with-a-repeater-control-vb.md)</span><span class="sxs-lookup"><span data-stu-id="06885-136">[Previous](positioning-a-modalpopup-cs.md)
[Next](using-modalpopup-with-a-repeater-control-vb.md)</span></span>
