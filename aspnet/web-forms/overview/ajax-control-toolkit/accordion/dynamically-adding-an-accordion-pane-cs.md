---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
title: Dynamisches Hinzufügen von einem des Accordion-Bereichs (c#) | Microsoft-Dokumentation
author: wenz
description: Das ' Accordion '-Steuerelement im AJAX Control Toolkit bietet mehrere Bereiche und ermöglicht dem Benutzer eine von ihnen zu einem Zeitpunkt angezeigt. Bereiche werden in der Regel deklariert, w...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 66d88cfa-f26f-46b1-ad52-1c9e03c04a48
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-cs
msc.type: authoredcontent
ms.openlocfilehash: 60bff8dd2d06356a1f2cc771cf5b7fa9c4e4eac5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37803248"
---
<a name="dynamically-adding-an-accordion-pane-c"></a><span data-ttu-id="732fe-104">Dynamisches Hinzufügen von einem des Accordion-Bereichs (c#)</span><span class="sxs-lookup"><span data-stu-id="732fe-104">Dynamically Adding An Accordion Pane (C#)</span></span>
====================
<span data-ttu-id="732fe-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="732fe-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="732fe-106">[Code herunterladen](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.cs.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="732fe-106">[Download Code](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.cs.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2CS.pdf)</span></span>

> <span data-ttu-id="732fe-107">Das ' Accordion '-Steuerelement im AJAX Control Toolkit bietet mehrere Bereiche und ermöglicht dem Benutzer eine von ihnen zu einem Zeitpunkt angezeigt.</span><span class="sxs-lookup"><span data-stu-id="732fe-107">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="732fe-108">Bereiche werden in der Regel auf der Seite selbst deklariert, aber von serverseitigem Code kann verwendet werden, um das gleiche Ergebnis zu erzielen.</span><span class="sxs-lookup"><span data-stu-id="732fe-108">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>


## <a name="overview"></a><span data-ttu-id="732fe-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="732fe-109">Overview</span></span>

<span data-ttu-id="732fe-110">Das ' Accordion '-Steuerelement im AJAX Control Toolkit bietet mehrere Bereiche und ermöglicht dem Benutzer eine von ihnen zu einem Zeitpunkt angezeigt.</span><span class="sxs-lookup"><span data-stu-id="732fe-110">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="732fe-111">Bereiche werden in der Regel auf der Seite selbst deklariert, aber von serverseitigem Code kann verwendet werden, um das gleiche Ergebnis zu erzielen.</span><span class="sxs-lookup"><span data-stu-id="732fe-111">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>

## <a name="steps"></a><span data-ttu-id="732fe-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="732fe-112">Steps</span></span>

<span data-ttu-id="732fe-113">Das Steuerelement ' Accordion ' macht alle wichtige Eigenschaften serverseitiger Code verfügbar.</span><span class="sxs-lookup"><span data-stu-id="732fe-113">The Accordion control exposes all important properties to server-side code.</span></span> <span data-ttu-id="732fe-114">Unter anderem die `Panes` Eigenschaft erhalten Sie Zugriff auf die Auflistung der Bereiche, die die Akkordeon bilden.</span><span class="sxs-lookup"><span data-stu-id="732fe-114">Among other things, the `Panes` property grants access to the collection of panes that make up the Accordion.</span></span> <span data-ttu-id="732fe-115">Jeder Bereich es vom Typ wird `AccordionPane`.</span><span class="sxs-lookup"><span data-stu-id="732fe-115">Every pane there is of type `AccordionPane`.</span></span> <span data-ttu-id="732fe-116">Daher ist es einfach, diese einen Bereich zu erstellen:</span><span class="sxs-lookup"><span data-stu-id="732fe-116">It is therefore trivial to create such a pane:</span></span>

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample1.cs)]

<span data-ttu-id="732fe-117">Die `HeaderContainer` Eigenschaft `AccordionPane` ermöglicht den Zugriff auf die ASP.NET-Steuerelemente in den Headerbereich des Bereichs; der `ContentContainer` Eigenschaft `AccordionPane` führt den gleichen Wert für den Content-Abschnitt des Bereichs.</span><span class="sxs-lookup"><span data-stu-id="732fe-117">The `HeaderContainer` property of `AccordionPane` provides access to the ASP.NET controls within the header section of the pane; the `ContentContainer` property of `AccordionPane` does the same for the content section of the pane.</span></span> <span data-ttu-id="732fe-118">Dadurch können ASP-Code, um die Bereiche Inhalt hinzuzufügen:</span><span class="sxs-lookup"><span data-stu-id="732fe-118">This allows ASP.NET code to add content to the panes:</span></span>

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample2.cs)]

<span data-ttu-id="732fe-119">Abschließend muss die Pane(s) hinzugefügt werden, um die `Panes` Auflistung von der ' Accordion ':</span><span class="sxs-lookup"><span data-stu-id="732fe-119">Finally, the pane(s) must be added to the `Panes` collection of the Accordion:</span></span>

[!code-csharp[Main](dynamically-adding-an-accordion-pane-cs/samples/sample3.cs)]

<span data-ttu-id="732fe-120">Hier ist ein vollständige serverseitigen Code, der zwei Bereiche ein Akkordeon-Steuerelement hinzufügt:</span><span class="sxs-lookup"><span data-stu-id="732fe-120">Here is a complete server-side code that adds two panes to an Accordion control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample4.aspx)]

<span data-ttu-id="732fe-121">Das einzige Element ist nicht vorhanden ist, die ' Accordion ' selbst, auf die das Vorhandensein von ASP.NET `ScriptManager` Steuerelement:</span><span class="sxs-lookup"><span data-stu-id="732fe-121">The only missing element is the Accordion itself, which depends on the presence of the ASP.NET `ScriptManager` control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-cs/samples/sample5.aspx)]

<span data-ttu-id="732fe-122">Um das Beispiel abgeschlossen haben, geben Sie die beiden CSS-Klassen, die im Steuerelement ' Accordion ' auf die verwiesen wird. Informationen zum Schriftschnitt für den Browser:</span><span class="sxs-lookup"><span data-stu-id="732fe-122">To finish the example, the two CSS classes referenced in the Accordion control provide style information for the browser:</span></span>

[!code-css[Main](dynamically-adding-an-accordion-pane-cs/samples/sample6.css)]


<span data-ttu-id="732fe-123">[![Die Daten in der ' Accordion ' wurde von serverseitigem Code dynamisch hinzugefügt.](dynamically-adding-an-accordion-pane-cs/_static/image2.png)](dynamically-adding-an-accordion-pane-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="732fe-123">[![The data in the accordion was dynamically added by server-side code](dynamically-adding-an-accordion-pane-cs/_static/image2.png)](dynamically-adding-an-accordion-pane-cs/_static/image1.png)</span></span>

<span data-ttu-id="732fe-124">Die Daten in der ' Accordion ' wurde vom serverseitigen Code dynamisch hinzugefügt ([klicken Sie, um das Bild in voller Größe anzeigen](dynamically-adding-an-accordion-pane-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="732fe-124">The data in the accordion was dynamically added by server-side code ([Click to view full-size image](dynamically-adding-an-accordion-pane-cs/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="732fe-125">[Zurück](databinding-to-an-accordion-cs.md)
> [Weiter](databinding-to-an-accordion-vb.md)</span><span class="sxs-lookup"><span data-stu-id="732fe-125">[Previous](databinding-to-an-accordion-cs.md)
[Next](databinding-to-an-accordion-vb.md)</span></span>
