---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
title: ReorderList (VB) mit Postbacks | Microsoft Docs
author: wenz
description: Das Steuerelement ReorderList im AJAX-Steuerelement-Toolkit bietet eine Liste, die vom Benutzer per Drag & Drop neu angeordnet werden kann. Wenn die Liste neu sortiert wird, eine Bestellung...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e5b6ed70-19ed-4024-ba4f-6d78e8acdc0f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
msc.type: authoredcontent
ms.openlocfilehash: ef43471f7d8cc94c1a82a368e27acef05f474f81
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="using-postbacks-with-reorderlist-vb"></a><span data-ttu-id="deea5-104">Verwenden von Postbacks mit ReorderList (VB)</span><span class="sxs-lookup"><span data-stu-id="deea5-104">Using Postbacks with ReorderList (VB)</span></span>
====================
<span data-ttu-id="deea5-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="deea5-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="deea5-106">[Herunterladen von Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.vb.zip) oder [PDF herunterladen](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="deea5-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4VB.pdf)</span></span>

> <span data-ttu-id="deea5-107">Das Steuerelement ReorderList im AJAX-Steuerelement-Toolkit bietet eine Liste, die vom Benutzer per Drag & Drop neu angeordnet werden kann.</span><span class="sxs-lookup"><span data-stu-id="deea5-107">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="deea5-108">Wenn die Liste neu sortiert wird, teilt ein Postback den Server über die Änderung.</span><span class="sxs-lookup"><span data-stu-id="deea5-108">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>


## <a name="overview"></a><span data-ttu-id="deea5-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="deea5-109">Overview</span></span>

<span data-ttu-id="deea5-110">Die `ReorderList` Steuerelement im AJAX-Steuerelement-Toolkit enthält eine Liste, die vom Benutzer per Drag & Drop neu angeordnet werden kann.</span><span class="sxs-lookup"><span data-stu-id="deea5-110">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="deea5-111">Wenn die Liste neu sortiert wird, teilt ein Postback den Server über die Änderung.</span><span class="sxs-lookup"><span data-stu-id="deea5-111">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>

## <a name="steps"></a><span data-ttu-id="deea5-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="deea5-112">Steps</span></span>

<span data-ttu-id="deea5-113">Es gibt mehrere mögliche Datenquellen für die `ReorderList` Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="deea5-113">There are several possible data sources for the `ReorderList` control.</span></span> <span data-ttu-id="deea5-114">Verwenden einer `XmlDataSource` Steuerelement:</span><span class="sxs-lookup"><span data-stu-id="deea5-114">One is to use an `XmlDataSource` control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample1.aspx)]

<span data-ttu-id="deea5-115">Um diesen XML-Code zum Binden einer `ReorderList` Postbacks-Steuerelement, und aktivieren, die folgenden Attribute müssen festgelegt werden:</span><span class="sxs-lookup"><span data-stu-id="deea5-115">In order to bind this XML to a `ReorderList` control and enable postbacks, the following attributes must be set:</span></span>

- <span data-ttu-id="deea5-116">`DataSourceID`: Die ID der Datenquelle</span><span class="sxs-lookup"><span data-stu-id="deea5-116">`DataSourceID`: The ID of the data source</span></span>
- <span data-ttu-id="deea5-117">`SortOrderField`: Die Eigenschaft Sortierungskriterium</span><span class="sxs-lookup"><span data-stu-id="deea5-117">`SortOrderField`: The property to sort by</span></span>
- <span data-ttu-id="deea5-118">`AllowReorder`: Angibt, ob der Benutzer auf die Listenelemente neu anordnen.</span><span class="sxs-lookup"><span data-stu-id="deea5-118">`AllowReorder`: Whether to allow the user to reorder the list elements</span></span>
- <span data-ttu-id="deea5-119">`PostBackOnReorder`: Angibt, ob einen Postback erstellen, wenn die Liste neu angeordnet ist</span><span class="sxs-lookup"><span data-stu-id="deea5-119">`PostBackOnReorder`: Whether to create a postback whenever the list is rearranged</span></span>

<span data-ttu-id="deea5-120">So sieht das entsprechende Markup für das Steuerelement aus:</span><span class="sxs-lookup"><span data-stu-id="deea5-120">Here is the appropriate markup for the control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample2.aspx)]

<span data-ttu-id="deea5-121">Innerhalb der `ReorderList` Steuerelement bestimmte Daten aus der Datenquelle kann gebunden werden, mithilfe der `Eval()` Methode:</span><span class="sxs-lookup"><span data-stu-id="deea5-121">Within the `ReorderList` control, specific data from the data source may be bound using the `Eval()` method:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample3.aspx)]

<span data-ttu-id="deea5-122">An einer beliebigen Stelle auf der Seite wird eine Bezeichnung die Informationen enthalten, bei der letzten Neuanordnen von aufgetreten ist:</span><span class="sxs-lookup"><span data-stu-id="deea5-122">At an arbitrary position on the page, a label will hold the information when the last reordering occurred:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample4.aspx)]

<span data-ttu-id="deea5-123">Diese Bezeichnung wird Text im serverseitigen Code behandeln das Postback eingetragen:</span><span class="sxs-lookup"><span data-stu-id="deea5-123">This label is filled with text in the server-side code, handling the postback:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample5.aspx)]

<span data-ttu-id="deea5-124">Schließlich, um die Funktionalität von ASP.NET AJAX und das Steuerelement-Toolkit aktivieren die `ScriptManager` Steuerelement muss auf der Seite platziert werden:</span><span class="sxs-lookup"><span data-stu-id="deea5-124">Finally, in order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put on the page:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample6.aspx)]


<span data-ttu-id="deea5-125">[![Jede Neuanordnen von löst einen postback](using-postbacks-with-reorderlist-vb/_static/image2.png)](using-postbacks-with-reorderlist-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="deea5-125">[![Each reordering triggers a postback](using-postbacks-with-reorderlist-vb/_static/image2.png)](using-postbacks-with-reorderlist-vb/_static/image1.png)</span></span>

<span data-ttu-id="deea5-126">Jede Neuanordnen von löst einen Postback ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-postbacks-with-reorderlist-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="deea5-126">Each reordering triggers a postback ([Click to view full-size image](using-postbacks-with-reorderlist-vb/_static/image3.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="deea5-127">[Zurück](drag-and-drop-via-reorderlist-cs.md)
> [Weiter](drag-and-drop-via-reorderlist-vb.md)</span><span class="sxs-lookup"><span data-stu-id="deea5-127">[Previous](drag-and-drop-via-reorderlist-cs.md)
[Next](drag-and-drop-via-reorderlist-vb.md)</span></span>
