---
uid: web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-vb
title: Reduzieren und Erweitern eines Bereichs über JavaScript (VB) | Microsoft-Dokumentation
author: wenz
description: Das CollapsiblePanel-Steuerelement in ASP.NET AJAX Control Toolkit einen Bereich erweitert und bietet die Möglichkeit, seinen Inhalt zu reduzieren und erweitern ihn ein...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 298789b4-2964-49f5-a0a8-d4dbeb9ff2c2
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-vb
msc.type: authoredcontent
ms.openlocfilehash: 1f80a6979966a887db0557b4f1b98570e10b1ab7
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832210"
---
<a name="collapsing-and-expanding-a-panel-from-javascript-vb"></a><span data-ttu-id="c47cb-103">Reduzieren und Erweitern eines Bereichs über JavaScript (VB)</span><span class="sxs-lookup"><span data-stu-id="c47cb-103">Collapsing and Expanding a Panel from JavaScript (VB)</span></span>
====================
<span data-ttu-id="c47cb-104">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="c47cb-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="c47cb-105">[Code herunterladen](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.vb.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="c47cb-105">[Download Code](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1VB.pdf)</span></span>

> <span data-ttu-id="c47cb-106">Das CollapsiblePanel-Steuerelement in ASP.NET AJAX Control Toolkit einen Bereich erweitert und bietet die Möglichkeit, dessen Inhalt reduzieren und erweitern es noch mal.</span><span class="sxs-lookup"><span data-stu-id="c47cb-106">The CollapsiblePanel control in the ASP.NET AJAX Control Toolkit extends a panel and provides it with the capability to collapse its contents and expand it again.</span></span> <span data-ttu-id="c47cb-107">Diese beiden Aktionen können auch mit benutzerdefinierten JavaScript-Code ausgelöst werden.</span><span class="sxs-lookup"><span data-stu-id="c47cb-107">These two actions can also be triggered from custom JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="c47cb-108">Übersicht</span><span class="sxs-lookup"><span data-stu-id="c47cb-108">Overview</span></span>

<span data-ttu-id="c47cb-109">Das CollapsiblePanel-Steuerelement in ASP.NET AJAX Control Toolkit einen Bereich erweitert und bietet die Möglichkeit, dessen Inhalt reduzieren und erweitern es noch mal.</span><span class="sxs-lookup"><span data-stu-id="c47cb-109">The CollapsiblePanel control in the ASP.NET AJAX Control Toolkit extends a panel and provides it with the capability to collapse its contents and expand it again.</span></span> <span data-ttu-id="c47cb-110">Diese beiden Aktionen können auch mit benutzerdefinierten JavaScript-Code ausgelöst werden.</span><span class="sxs-lookup"><span data-stu-id="c47cb-110">These two actions can also be triggered from custom JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="c47cb-111">Schritte</span><span class="sxs-lookup"><span data-stu-id="c47cb-111">Steps</span></span>

<span data-ttu-id="c47cb-112">Zunächst erstellen Sie eine neue ASP.NET-Seite und enthalten die `ScriptManager` innerhalb der `<form>` Element.</span><span class="sxs-lookup"><span data-stu-id="c47cb-112">First of all, create a new ASP.NET page and include the `ScriptManager` within the one `<form>` element.</span></span> <span data-ttu-id="c47cb-113">Dies lädt die ASP.NET AJAX-Bibliothek, die für das Steuerelement-Toolkit erforderlich ist:</span><span class="sxs-lookup"><span data-stu-id="c47cb-113">This loads the ASP.NET AJAX library which is required by the Control Toolkit:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample1.aspx)]

<span data-ttu-id="c47cb-114">Erstellen Sie Sie dann ein Panel mit Text, damit die Auswirkungen Reduzieren/Erweitern angezeigt werden:</span><span class="sxs-lookup"><span data-stu-id="c47cb-114">Then, create a panel with some text so that the collapse/expand effect can be seen:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample2.aspx)]

<span data-ttu-id="c47cb-115">Wie Sie sehen können, verweist der Bereich eine CSS-Klasse, die wird hier gezeigt (und werden im Grunde eine Hintergrundfarbe und des Bereichs Breite definiert):</span><span class="sxs-lookup"><span data-stu-id="c47cb-115">As you can see, the panel references a CSS class which is shown here (and basically defines a background color and the panel's width):</span></span>

[!code-css[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample3.css)]

<span data-ttu-id="c47cb-116">Die `CollapsiblePanelExtender` Steuerelement erfordert die `TargetControlID` Attribut, damit das Toolkit weiß, welchen Bereich reduzieren oder Erweitern auf Anfrage:</span><span class="sxs-lookup"><span data-stu-id="c47cb-116">The `CollapsiblePanelExtender` control requires the `TargetControlID` attribute so that the toolkit knows which panel to collapse or expand upon request:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample4.aspx)]

<span data-ttu-id="c47cb-117">Leider wird der Extender derzeit macht eine bestimmte API für den Bereich zu erweitern oder reduzieren, aber einige nicht dokumentierten Methoden erfolgt.</span><span class="sxs-lookup"><span data-stu-id="c47cb-117">Unfortunately, the extender currently does not expose a specific API for collapsing or expanding the panel, but some undocumented methods will do.</span></span> <span data-ttu-id="c47cb-118">Fügen Sie zunächst auf die Seite, die dann auslösen, wird die clientseitige JavaScript-Code zum Reduzieren oder erweitern den Inhalt des Bereichs drei HTML-Schaltflächen hinzu:</span><span class="sxs-lookup"><span data-stu-id="c47cb-118">First of all, add three HTML buttons to the page which will then trigger the client-side JavaScript to collapse or expand the panel's contents:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample5.aspx)]

<span data-ttu-id="c47cb-119">Im clientseitigen JavaScript-Code (Einstieg `<script type="text/javascript">`), wird die `$find()` -Methode verwendet werden, muss für den Zugriff auf die `CollapsiblePanelExtender`.</span><span class="sxs-lookup"><span data-stu-id="c47cb-119">In the client-side JavaScript code (started with `<script type="text/javascript">`), the `$find()` method needs to be used to access the `CollapsiblePanelExtender`.</span></span> <span data-ttu-id="c47cb-120">`$find("cpe")` Gibt einen Verweis darauf zurück.</span><span class="sxs-lookup"><span data-stu-id="c47cb-120">`$find("cpe")` will return a reference to it.</span></span> <span data-ttu-id="c47cb-121">Von dort auf bestimmte Methoden lösen die aktuelle Aufgabe.</span><span class="sxs-lookup"><span data-stu-id="c47cb-121">From there on, specific methods will solve the task at hand.</span></span>

<span data-ttu-id="c47cb-122">Die Methode zum Öffnen von (Erweitert) im Bereich heißt `_doOpen()`; der folgende code implementiert die `doOpen()` Funktion aufgerufen, wenn die erste Schaltfläche geklickt wird:</span><span class="sxs-lookup"><span data-stu-id="c47cb-122">The method for opening (expanding) the panel is called `_doOpen()`; the following code implements the `doOpen()` function called when the first button is clicked:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample6.js)]

<span data-ttu-id="c47cb-123">Zum Schließen, oder reduzieren im Bereich der `_doClose()` -Methode ausgeführt werden muss.</span><span class="sxs-lookup"><span data-stu-id="c47cb-123">For closing, or collapsing the panel, the `_doClose()` method needs to be executed.</span></span> <span data-ttu-id="c47cb-124">Wenn der Benutzer auf der zweiten Schaltfläche klickt, wird also der folgende JavaScript-Code aufgerufen:</span><span class="sxs-lookup"><span data-stu-id="c47cb-124">So when the user clicks on the second button, the following JavaScript code is called:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample7.js)]

<span data-ttu-id="c47cb-125">Die dritte Schaltfläche Schaltet den Zustand des Bereichs: aus reduziert, um erweitert, und umgekehrt.</span><span class="sxs-lookup"><span data-stu-id="c47cb-125">The third button toggles the state of the panel: from collapsed to expanded, and vice versa.</span></span> <span data-ttu-id="c47cb-126">Die `CollapsiblePanelExtender` macht die `toggle()` -Methode die genau diese Lücke: Kehrt den Zustand des Bereichs.</span><span class="sxs-lookup"><span data-stu-id="c47cb-126">The `CollapsiblePanelExtender` exposes the `toggle()` method which does exactly that: reverses the state of the panel.</span></span> <span data-ttu-id="c47cb-127">Aber es gibt auch ein anderer Ansatz (wird intern von verwendet die `toggle()` Methode): der `get_Collapsed()` -Methode der der `CollapsiblePanelExtender()` gibt an, ob der Bereich reduziert wird oder nicht.</span><span class="sxs-lookup"><span data-stu-id="c47cb-127">However there is also another approach (which is internally used by the `toggle()` method): The `get_Collapsed()` method of the `CollapsiblePanelExtender()` tells us whether the panel is collapsed or not.</span></span> <span data-ttu-id="c47cb-128">Je nach den von dieser Funktion zurückgegebenen Wert, der Bereich ist, und klicken Sie dann entweder erweitert (`_doOpen()` Methode) oder reduziert (`_doClose()`) Methode:</span><span class="sxs-lookup"><span data-stu-id="c47cb-128">Depending on the return value of this function, the panel is then either expanded (`_doOpen()` method) or collapsed (`_doClose()`) method:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample8.js)]


<span data-ttu-id="c47cb-129">[![Die dritte Schaltfläche ändert den Zustand des Bereichs: aus reduziert erweiterte und zurück](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="c47cb-129">[![The third button changes the state of the panel: from collapsed to expanded and back](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image1.png)</span></span>

<span data-ttu-id="c47cb-130">Die dritte Schaltfläche ändert den Zustand des Bereichs: aus reduziert erweiterte und zurück ([klicken Sie, um das Bild in voller Größe anzeigen](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="c47cb-130">The third button changes the state of the panel: from collapsed to expanded and back ([Click to view full-size image](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="c47cb-131">Vorherige</span><span class="sxs-lookup"><span data-stu-id="c47cb-131">Previous</span></span>](collapsing-and-expanding-a-panel-from-javascript-cs.md)
