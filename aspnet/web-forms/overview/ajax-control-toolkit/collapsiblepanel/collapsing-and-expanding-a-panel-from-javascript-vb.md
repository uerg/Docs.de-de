---
uid: web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-vb
title: Reduzieren und erweitern ein Panel aus JavaScript (VB) | Microsoft Docs
author: wenz
description: "Das CollapsiblePanel-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit einen Bereich erweitert und bietet die Möglichkeit, den Inhalt zu reduzieren und erweitern ihn ein..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 298789b4-2964-49f5-a0a8-d4dbeb9ff2c2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/collapsiblepanel/collapsing-and-expanding-a-panel-from-javascript-vb
msc.type: authoredcontent
ms.openlocfilehash: 6adca6771042cad71139977496f985cb8dac63aa
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="collapsing-and-expanding-a-panel-from-javascript-vb"></a><span data-ttu-id="5b6a1-103">Reduzieren und erweitern ein Panel aus JavaScript (VB)</span><span class="sxs-lookup"><span data-stu-id="5b6a1-103">Collapsing and Expanding a Panel from JavaScript (VB)</span></span>
====================
<span data-ttu-id="5b6a1-104">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="5b6a1-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="5b6a1-105">[Herunterladen von Code](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.vb.zip) oder [PDF herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="5b6a1-105">[Download Code](http://download.microsoft.com/download/8/a/a/8aab3c3e-de6f-463f-805c-5fda567eef6e/CollapsiblePanel1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/collapsiblepanel1VB.pdf)</span></span>

> <span data-ttu-id="5b6a1-106">Das CollapsiblePanel-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit einen Bereich erweitert und bietet die Möglichkeit, seinen Inhalt zu reduzieren und erweitern ihn erneut.</span><span class="sxs-lookup"><span data-stu-id="5b6a1-106">The CollapsiblePanel control in the ASP.NET AJAX Control Toolkit extends a panel and provides it with the capability to collapse its contents and expand it again.</span></span> <span data-ttu-id="5b6a1-107">Diese zwei Aktionen können auch benutzerdefinierte JavaScript-Code ausgelöst werden.</span><span class="sxs-lookup"><span data-stu-id="5b6a1-107">These two actions can also be triggered from custom JavaScript code.</span></span>


## <a name="overview"></a><span data-ttu-id="5b6a1-108">Übersicht</span><span class="sxs-lookup"><span data-stu-id="5b6a1-108">Overview</span></span>

<span data-ttu-id="5b6a1-109">Das CollapsiblePanel-Steuerelement in ASP.NET AJAX-Steuerelement-Toolkit einen Bereich erweitert und bietet die Möglichkeit, seinen Inhalt zu reduzieren und erweitern ihn erneut.</span><span class="sxs-lookup"><span data-stu-id="5b6a1-109">The CollapsiblePanel control in the ASP.NET AJAX Control Toolkit extends a panel and provides it with the capability to collapse its contents and expand it again.</span></span> <span data-ttu-id="5b6a1-110">Diese zwei Aktionen können auch benutzerdefinierte JavaScript-Code ausgelöst werden.</span><span class="sxs-lookup"><span data-stu-id="5b6a1-110">These two actions can also be triggered from custom JavaScript code.</span></span>

## <a name="steps"></a><span data-ttu-id="5b6a1-111">Schritte</span><span class="sxs-lookup"><span data-stu-id="5b6a1-111">Steps</span></span>

<span data-ttu-id="5b6a1-112">Zunächst erstellen Sie eine neue ASP.NET-Seite und enthalten die `ScriptManager` innerhalb der `<form>` Element.</span><span class="sxs-lookup"><span data-stu-id="5b6a1-112">First of all, create a new ASP.NET page and include the `ScriptManager` within the one `<form>` element.</span></span> <span data-ttu-id="5b6a1-113">Dadurch wird geladen, die ASP.NET AJAX-Bibliothek, die vom Toolkit-Steuerelement erforderlich ist:</span><span class="sxs-lookup"><span data-stu-id="5b6a1-113">This loads the ASP.NET AJAX library which is required by the Control Toolkit:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample1.aspx)]

<span data-ttu-id="5b6a1-114">Dann erstellen Sie ein Panel mit Text, damit die Auswirkungen Reduzieren/Erweitern angezeigt werden kann:</span><span class="sxs-lookup"><span data-stu-id="5b6a1-114">Then, create a panel with some text so that the collapse/expand effect can be seen:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample2.aspx)]

<span data-ttu-id="5b6a1-115">Wie Sie sehen können, verweist im Bereich auf eine CSS-Klasse, die hier gezeigt wird (und im Grunde eine Hintergrundfarbe und Breite des Bereichs definiert):</span><span class="sxs-lookup"><span data-stu-id="5b6a1-115">As you can see, the panel references a CSS class which is shown here (and basically defines a background color and the panel's width):</span></span>

[!code-css[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample3.css)]

<span data-ttu-id="5b6a1-116">Die `CollapsiblePanelExtender` Steuerelement erfordert die `TargetControlID` Attribut, damit das Toolkit weiß, welche Bereich zu reduzieren oder erweitern, auf Anforderung:</span><span class="sxs-lookup"><span data-stu-id="5b6a1-116">The `CollapsiblePanelExtender` control requires the `TargetControlID` attribute so that the toolkit knows which panel to collapse or expand upon request:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample4.aspx)]

<span data-ttu-id="5b6a1-117">Leider der Extender derzeit nicht verfügbar macht eine bestimmte API für reduzieren oder Erweitern im Bereich, aber einige nicht dokumentierten Methoden führen.</span><span class="sxs-lookup"><span data-stu-id="5b6a1-117">Unfortunately, the extender currently does not expose a specific API for collapsing or expanding the panel, but some undocumented methods will do.</span></span> <span data-ttu-id="5b6a1-118">Erstens Hinzufügen von drei HTML-Schaltflächen auf der Seite klicken Sie dann die clientseitiges JavaScript zum Reduzieren oder erweitern den Inhalt des Bereichs auslösen:</span><span class="sxs-lookup"><span data-stu-id="5b6a1-118">First of all, add three HTML buttons to the page which will then trigger the client-side JavaScript to collapse or expand the panel's contents:</span></span>

[!code-aspx[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample5.aspx)]

<span data-ttu-id="5b6a1-119">Im clientseitigen JavaScript-Code (Einstieg `<script type="text/javascript">`), die `$find()` Methode verwendet werden, muss für den Zugriff auf die `CollapsiblePanelExtender`.</span><span class="sxs-lookup"><span data-stu-id="5b6a1-119">In the client-side JavaScript code (started with `<script type="text/javascript">`), the `$find()` method needs to be used to access the `CollapsiblePanelExtender`.</span></span> <span data-ttu-id="5b6a1-120">`$find("cpe")`Gibt einen Verweis darauf zurück.</span><span class="sxs-lookup"><span data-stu-id="5b6a1-120">`$find("cpe")` will return a reference to it.</span></span> <span data-ttu-id="5b6a1-121">Von dort auf bestimmte Methoden lösen die Aufgabe.</span><span class="sxs-lookup"><span data-stu-id="5b6a1-121">From there on, specific methods will solve the task at hand.</span></span>

<span data-ttu-id="5b6a1-122">Die Methode zum Öffnen von (Erweitert) wird im Bereich aufgerufen `_doOpen()`; der folgende code implementiert die `doOpen()` Funktion aufgerufen, wenn die erste Schaltfläche geklickt wird:</span><span class="sxs-lookup"><span data-stu-id="5b6a1-122">The method for opening (expanding) the panel is called `_doOpen()`; the following code implements the `doOpen()` function called when the first button is clicked:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample6.js)]

<span data-ttu-id="5b6a1-123">Für das Schließen, oder reduzieren den Bereich der `_doClose()` Methode ausgeführt werden muss.</span><span class="sxs-lookup"><span data-stu-id="5b6a1-123">For closing, or collapsing the panel, the `_doClose()` method needs to be executed.</span></span> <span data-ttu-id="5b6a1-124">Der Benutzer auf die zweite Schaltfläche klickt, wird daher folgende JavaScript-Code aufgerufen:</span><span class="sxs-lookup"><span data-stu-id="5b6a1-124">So when the user clicks on the second button, the following JavaScript code is called:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample7.js)]

<span data-ttu-id="5b6a1-125">Die dritte Schaltfläche Schaltet den Zustand des Bereichs: aus reduziert erweitert, und umgekehrt.</span><span class="sxs-lookup"><span data-stu-id="5b6a1-125">The third button toggles the state of the panel: from collapsed to expanded, and vice versa.</span></span> <span data-ttu-id="5b6a1-126">Die `CollapsiblePanelExtender` macht die `toggle()` Methode was genau dies der Fall ist: Kehrt den Zustand des Bereichs.</span><span class="sxs-lookup"><span data-stu-id="5b6a1-126">The `CollapsiblePanelExtender` exposes the `toggle()` method which does exactly that: reverses the state of the panel.</span></span> <span data-ttu-id="5b6a1-127">Aber es gibt auch ein anderer Ansatz (der intern von verwendet wird die `toggle()` Methode): der `get_Collapsed()` Methode der `CollapsiblePanelExtender()` gibt an, ob der Bereich reduziert wird.</span><span class="sxs-lookup"><span data-stu-id="5b6a1-127">However there is also another approach (which is internally used by the `toggle()` method): The `get_Collapsed()` method of the `CollapsiblePanelExtender()` tells us whether the panel is collapsed or not.</span></span> <span data-ttu-id="5b6a1-128">Abhängig vom Rückgabewert dieser Funktion, der Bereich ist, und klicken Sie dann entweder erweitert (`_doOpen()` Methode) oder reduzierter Form (`_doClose()`) Methode:</span><span class="sxs-lookup"><span data-stu-id="5b6a1-128">Depending on the return value of this function, the panel is then either expanded (`_doOpen()` method) or collapsed (`_doClose()`) method:</span></span>

[!code-javascript[Main](collapsing-and-expanding-a-panel-from-javascript-vb/samples/sample8.js)]


<span data-ttu-id="5b6a1-129">[![Die dritte Schaltfläche ändert den Zustand des Bereichs: aus reduziert erweitert und zurück](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="5b6a1-129">[![The third button changes the state of the panel: from collapsed to expanded and back](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image2.png)](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image1.png)</span></span>

<span data-ttu-id="5b6a1-130">Die dritte Schaltfläche ändert den Zustand des Bereichs: aus reduziert erweitert und zurück ([klicken Sie hier, um das Bild in voller Größe angezeigt](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="5b6a1-130">The third button changes the state of the panel: from collapsed to expanded and back ([Click to view full-size image](collapsing-and-expanding-a-panel-from-javascript-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="5b6a1-131">Zurück</span><span class="sxs-lookup"><span data-stu-id="5b6a1-131">Previous</span></span>](collapsing-and-expanding-a-panel-from-javascript-cs.md)
