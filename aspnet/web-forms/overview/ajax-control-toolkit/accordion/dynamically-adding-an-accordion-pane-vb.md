---
uid: web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
title: Dynamisches Hinzufügen von einem Akkordeon Bereich (VB) | Microsoft Docs
author: wenz
description: Das Steuerelement ' Accordion ' im AJAX-Steuerelement-Toolkit enthält mehrere Bereiche und ermöglicht es dem Benutzer eines davon zu einem Zeitpunkt angezeigt. Bereiche werden in der Regel w deklariert...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: fae968c9-1902-487d-b053-86a46dd52c3f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/accordion/dynamically-adding-an-accordion-pane-vb
msc.type: authoredcontent
ms.openlocfilehash: 68c60ba6d4be5eb6709f7558d6be4165f8232a4f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="dynamically-adding-an-accordion-pane-vb"></a><span data-ttu-id="ffb9f-104">Dynamisches Hinzufügen von einem Akkordeon Bereich (VB)</span><span class="sxs-lookup"><span data-stu-id="ffb9f-104">Dynamically Adding An Accordion Pane (VB)</span></span>
====================
<span data-ttu-id="ffb9f-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="ffb9f-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="ffb9f-106">[Herunterladen von Code](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) oder [PDF herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="ffb9f-106">[Download Code](http://download.microsoft.com/download/5/6/d/56d50cef-2011-4c8f-9891-7edc6dc57df9/Accordion2.vb.zip) or [Download PDF](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/accordion2VB.pdf)</span></span>

> <span data-ttu-id="ffb9f-107">Das Steuerelement ' Accordion ' im AJAX-Steuerelement-Toolkit enthält mehrere Bereiche und ermöglicht es dem Benutzer eines davon zu einem Zeitpunkt angezeigt.</span><span class="sxs-lookup"><span data-stu-id="ffb9f-107">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="ffb9f-108">Bereiche werden in der Regel auf der Seite selbst deklariert, aber serverseitigen Code kann verwendet werden, um das gleiche Ergebnis zu erzielen.</span><span class="sxs-lookup"><span data-stu-id="ffb9f-108">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>


## <a name="overview"></a><span data-ttu-id="ffb9f-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="ffb9f-109">Overview</span></span>

<span data-ttu-id="ffb9f-110">Das Steuerelement ' Accordion ' im AJAX-Steuerelement-Toolkit enthält mehrere Bereiche und ermöglicht es dem Benutzer eines davon zu einem Zeitpunkt angezeigt.</span><span class="sxs-lookup"><span data-stu-id="ffb9f-110">The Accordion control in the AJAX Control Toolkit provides multiple panes and allows the user to display one of them at a time.</span></span> <span data-ttu-id="ffb9f-111">Bereiche werden in der Regel auf der Seite selbst deklariert, aber serverseitigen Code kann verwendet werden, um das gleiche Ergebnis zu erzielen.</span><span class="sxs-lookup"><span data-stu-id="ffb9f-111">Panels are usually declared within the page itself, but server-side code can be used to achieve the same result.</span></span>

## <a name="steps"></a><span data-ttu-id="ffb9f-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="ffb9f-112">Steps</span></span>

<span data-ttu-id="ffb9f-113">Das Steuerelement ' Accordion ' macht alle wichtige Eigenschaften mit serverseitigen Code verfügbar.</span><span class="sxs-lookup"><span data-stu-id="ffb9f-113">The Accordion control exposes all important properties to server-side code.</span></span> <span data-ttu-id="ffb9f-114">Unter anderem die `Panes` Eigenschaft gewährt Zugriff auf die Auflistung der Bereiche, die die ' Accordion ' bilden.</span><span class="sxs-lookup"><span data-stu-id="ffb9f-114">Among other things, the `Panes` property grants access to the collection of panes that make up the Accordion.</span></span> <span data-ttu-id="ffb9f-115">Jeder Bereich es vom Typ ist `AccordionPane`.</span><span class="sxs-lookup"><span data-stu-id="ffb9f-115">Every pane there is of type `AccordionPane`.</span></span> <span data-ttu-id="ffb9f-116">Es ist daher einfach einen Bereich im erstellen:</span><span class="sxs-lookup"><span data-stu-id="ffb9f-116">It is therefore trivial to create such a pane:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample1.vb)]

<span data-ttu-id="ffb9f-117">Die `HeaderContainer` Eigenschaft `AccordionPane` ermöglicht den Zugriff auf den ASP.NET-Steuerelementen innerhalb der Headerabschnitt eines Bereich; die `ContentContainer` Eigenschaft `AccordionPane` erfüllt den gleichen Wert für den Inhaltsabschnitt des Bereichs.</span><span class="sxs-lookup"><span data-stu-id="ffb9f-117">The `HeaderContainer` property of `AccordionPane` provides access to the ASP.NET controls within the header section of the pane; the `ContentContainer` property of `AccordionPane` does the same for the content section of the pane.</span></span> <span data-ttu-id="ffb9f-118">Dadurch können ASP-Code, um die Bereiche Inhalt hinzuzufügen:</span><span class="sxs-lookup"><span data-stu-id="ffb9f-118">This allows ASP.NET code to add content to the panes:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample2.vb)]

<span data-ttu-id="ffb9f-119">Schließlich muss die Pane(s) hinzugefügt werden, um die `Panes` Auflistung von der ' Accordion ':</span><span class="sxs-lookup"><span data-stu-id="ffb9f-119">Finally, the pane(s) must be added to the `Panes` collection of the Accordion:</span></span>

[!code-vb[Main](dynamically-adding-an-accordion-pane-vb/samples/sample3.vb)]

<span data-ttu-id="ffb9f-120">Hier ist ein vollständige serverseitigen Code, der zwei Bereiche auf das Steuerelement ' Accordion ' hinzufügt:</span><span class="sxs-lookup"><span data-stu-id="ffb9f-120">Here is a complete server-side code that adds two panes to an Accordion control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample4.aspx)]

<span data-ttu-id="ffb9f-121">Das einzige fehlende Element ist das ' Accordion ' selbst, abhängig von der Anwesenheit von ASP.NET `ScriptManager` Steuerelement:</span><span class="sxs-lookup"><span data-stu-id="ffb9f-121">The only missing element is the Accordion itself, which depends on the presence of the ASP.NET `ScriptManager` control:</span></span>

[!code-aspx[Main](dynamically-adding-an-accordion-pane-vb/samples/sample5.aspx)]

<span data-ttu-id="ffb9f-122">Um das Beispiel fertig sind, enthalten die beiden CSS-Klassen verwiesen wird, in das Steuerelement ' Accordion ' Formatinformationen für den Browser:</span><span class="sxs-lookup"><span data-stu-id="ffb9f-122">To finish the example, the two CSS classes referenced in the Accordion control provide style information for the browser:</span></span>

[!code-css[Main](dynamically-adding-an-accordion-pane-vb/samples/sample6.css)]


<span data-ttu-id="ffb9f-123">[![Die Daten in der ' Accordion ' wurde vom serverseitigen Code dynamisch hinzugefügt.](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="ffb9f-123">[![The data in the accordion was dynamically added by server-side code](dynamically-adding-an-accordion-pane-vb/_static/image2.png)](dynamically-adding-an-accordion-pane-vb/_static/image1.png)</span></span>

<span data-ttu-id="ffb9f-124">Die Daten in der ' Accordion ' wurde vom serverseitigen Code dynamisch hinzugefügt ([klicken Sie hier, um das Bild in voller Größe angezeigt](dynamically-adding-an-accordion-pane-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="ffb9f-124">The data in the accordion was dynamically added by server-side code ([Click to view full-size image](dynamically-adding-an-accordion-pane-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="ffb9f-125">Vorherige</span><span class="sxs-lookup"><span data-stu-id="ffb9f-125">Previous</span></span>](databinding-to-an-accordion-vb.md)
