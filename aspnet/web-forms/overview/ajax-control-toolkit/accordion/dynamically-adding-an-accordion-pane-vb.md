---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
title: Dynamisches Hinzufügen von einem des Accordion-Bereichs (VB) | Microsoft-Dokumentation
author: wenz
description: Das ' Accordion '-Steuerelement im AJAX Control Toolkit bietet mehrere Bereiche und ermöglicht dem Benutzer eine von ihnen zu einem Zeitpunkt angezeigt. Bereiche werden in der Regel deklariert, w...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: fae968c9-1902-487d-b053-86a46dd52c3f
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
msc.type: authoredcontent
ms.openlocfilehash: 3dd82fab03e06aa5dd3baba7dd24734fa964b350
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37378961"
---
<a name="dynamically-adding-an-accordion-pane-vb"></a><span data-ttu-id="e7658-104">Dynamisches Hinzufügen von einem des Accordion-Bereichs (VB)</span><span class="sxs-lookup"><span data-stu-id="e7658-104">Dynamically Adding An Accordion Pane (VB)</span></span>
====================
<span data-ttu-id="e7658-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="e7658-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="e7658-106">[Code herunterladen](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="e7658-106">[Download Code](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)</span></span>

> <span data-ttu-id="e7658-107">Das ' Accordion '-Steuerelement im AJAX Control Toolkit bietet mehrere Bereiche und ermöglicht dem Benutzer eine von ihnen zu einem Zeitpunkt angezeigt.</span><span class="sxs-lookup"><span data-stu-id="e7658-107">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="e7658-108">Bereiche werden in der Regel auf der Seite selbst deklariert, aber von serverseitigem Code kann verwendet werden, um das gleiche Ergebnis zu erzielen.</span><span class="sxs-lookup"><span data-stu-id="e7658-108">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>


## <a name="overview"></a><span data-ttu-id="e7658-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="e7658-109">Overview</span></span>

<span data-ttu-id="e7658-110">Das ' Accordion '-Steuerelement im AJAX Control Toolkit bietet mehrere Bereiche und ermöglicht dem Benutzer eine von ihnen zu einem Zeitpunkt angezeigt.</span><span class="sxs-lookup"><span data-stu-id="e7658-110">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="e7658-111">Bereiche werden in der Regel auf der Seite selbst deklariert, aber von serverseitigem Code kann verwendet werden, um das gleiche Ergebnis zu erzielen.</span><span class="sxs-lookup"><span data-stu-id="e7658-111">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>

## <a name="steps"></a><span data-ttu-id="e7658-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="e7658-112">Steps</span></span>

<span data-ttu-id="e7658-113">Das Steuerelement ' Accordion ' macht alle wichtige Eigenschaften serverseitiger Code verfügbar.</span><span class="sxs-lookup"><span data-stu-id="e7658-113">The Accordion control exposes all important properties to server-side code.</span></span> <span data-ttu-id="e7658-114">Unter anderem die `Panes` Eigenschaft erhalten Sie Zugriff auf die Auflistung der Bereiche, die die Akkordeon bilden.</span><span class="sxs-lookup"><span data-stu-id="e7658-114">Among other things, the `Panes` property grants access to the collection of panes that make up the Accordion.</span></span> <span data-ttu-id="e7658-115">Jeder Bereich es vom Typ wird `AccordionPane`.</span><span class="sxs-lookup"><span data-stu-id="e7658-115">Every pane there is of type `AccordionPane`.</span></span> <span data-ttu-id="e7658-116">Daher ist es einfach, diese einen Bereich zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="e7658-116">It is therefore trivial to create such a pane:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample1.vb)]

<span data-ttu-id="e7658-117">Die `HeaderContainer` Eigenschaft `AccordionPane` ermöglicht den Zugriff auf die ASP.NET-Steuerelemente in den Headerbereich des Bereichs; der `ContentContainer` Eigenschaft `AccordionPane` führt den gleichen Wert für den Content-Abschnitt des Bereichs.</span><span class="sxs-lookup"><span data-stu-id="e7658-117">The `HeaderContainer` property of `AccordionPane` provides access to the ASP.NET controls within the header section of the pane; the `ContentContainer` property of `AccordionPane` does the same for the content section of the pane.</span></span> <span data-ttu-id="e7658-118">Dadurch können ASP-Code, um die Bereiche Inhalt hinzuzufügen:</span><span class="sxs-lookup"><span data-stu-id="e7658-118">This allows ASP.NET code to add content to the panes:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample2.vb)]

<span data-ttu-id="e7658-119">Abschließend muss die Pane(s) hinzugefügt werden, um die `Panes` Auflistung von der ' Accordion ':</span><span class="sxs-lookup"><span data-stu-id="e7658-119">Finally, the pane(s) must be added to the `Panes` collection of the Accordion:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample3.vb)]

<span data-ttu-id="e7658-120">Hier ist ein vollständige serverseitigen Code, der zwei Bereiche ein Akkordeon-Steuerelement hinzufügt:</span><span class="sxs-lookup"><span data-stu-id="e7658-120">Here is a complete server-side code that adds two panes to an Accordion control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample4.aspx)]

<span data-ttu-id="e7658-121">Das einzige Element ist nicht vorhanden ist, die ' Accordion ' selbst, auf die das Vorhandensein von ASP.NET `ScriptManager` Steuerelement:</span><span class="sxs-lookup"><span data-stu-id="e7658-121">The only missing element is the Accordion itself, which depends on the presence of the ASP.NET `ScriptManager` control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample5.aspx)]

<span data-ttu-id="e7658-122">Um das Beispiel abgeschlossen haben, geben Sie die beiden CSS-Klassen, die im Steuerelement ' Accordion ' auf die verwiesen wird. Informationen zum Schriftschnitt für den Browser:</span><span class="sxs-lookup"><span data-stu-id="e7658-122">To finish the example, the two CSS classes referenced in the Accordion control provide style information for the browser:</span></span>

[!code-css[Main](dynamically-adding-an-accordion-pane-vb/samples/sample6.css)]


<span data-ttu-id="e7658-123">[![Die Daten in der ' Accordion ' wurde von serverseitigem Code dynamisch hinzugefügt.](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="e7658-123">[![The data in the accordion was dynamically added by server-side code](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)</span></span>

<span data-ttu-id="e7658-124">Die Daten in der ' Accordion ' wurde vom serverseitigen Code dynamisch hinzugefügt ([klicken Sie, um das Bild in voller Größe anzeigen](dynamically-adding-an-accordion-pane-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="e7658-124">The data in the accordion was dynamically added by server-side code ([Click to view full-size image](dynamically-adding-an-accordion-pane-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="e7658-125">Vorherige</span><span class="sxs-lookup"><span data-stu-id="e7658-125">Previous</span></span>](databinding-to-an-accordion-vb.md)
