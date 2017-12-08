---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
title: ReorderList (c#) Postbacks mit | Microsoft Docs
author: wenz
description: Das Steuerelement ReorderList im AJAX-Steuerelement-Toolkit bietet eine Liste, die vom Benutzer per Drag & Drop neu angeordnet werden kann. Wenn die Liste neu sortiert wird, eine Bestellung...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 70d5d106-b547-442c-a7fd-3492b3e3d646
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-cs
msc.type: authoredcontent
ms.openlocfilehash: 5f5c5e253f6d85203a488152c5ad908157e6ee0c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="using-postbacks-with-reorderlist-c"></a><span data-ttu-id="5ab2d-104">Verwenden von Postbacks mit ReorderList (c#)</span><span class="sxs-lookup"><span data-stu-id="5ab2d-104">Using Postbacks with ReorderList (C#)</span></span>
====================
<span data-ttu-id="5ab2d-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="5ab2d-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="5ab2d-106">[Herunterladen von Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.cs.zip) oder [PDF herunterladen](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="5ab2d-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4CS.pdf)</span></span>

> <span data-ttu-id="5ab2d-107">Das Steuerelement ReorderList im AJAX-Steuerelement-Toolkit bietet eine Liste, die vom Benutzer per Drag & Drop neu angeordnet werden kann.</span><span class="sxs-lookup"><span data-stu-id="5ab2d-107">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="5ab2d-108">Wenn die Liste neu sortiert wird, teilt ein Postback den Server über die Änderung.</span><span class="sxs-lookup"><span data-stu-id="5ab2d-108">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>


## <a name="overview"></a><span data-ttu-id="5ab2d-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="5ab2d-109">Overview</span></span>

<span data-ttu-id="5ab2d-110">Die `ReorderList` Steuerelement im AJAX-Steuerelement-Toolkit enthält eine Liste, die vom Benutzer per Drag & Drop neu angeordnet werden kann.</span><span class="sxs-lookup"><span data-stu-id="5ab2d-110">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="5ab2d-111">Wenn die Liste neu sortiert wird, teilt ein Postback den Server über die Änderung.</span><span class="sxs-lookup"><span data-stu-id="5ab2d-111">Whenever the list is reordered, a postback shall inform the server of the change.</span></span>

## <a name="steps"></a><span data-ttu-id="5ab2d-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="5ab2d-112">Steps</span></span>

<span data-ttu-id="5ab2d-113">Es gibt mehrere mögliche Datenquellen für die `ReorderList` Steuerelement.</span><span class="sxs-lookup"><span data-stu-id="5ab2d-113">There are several possible data sources for the `ReorderList` control.</span></span> <span data-ttu-id="5ab2d-114">Verwenden einer `XmlDataSource` Steuerelement:</span><span class="sxs-lookup"><span data-stu-id="5ab2d-114">One is to use an `XmlDataSource` control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample1.aspx)]

<span data-ttu-id="5ab2d-115">Um diesen XML-Code zum Binden einer `ReorderList` Postbacks-Steuerelement, und aktivieren, die folgenden Attribute müssen festgelegt werden:</span><span class="sxs-lookup"><span data-stu-id="5ab2d-115">In order to bind this XML to a `ReorderList` control and enable postbacks, the following attributes must be set:</span></span>

- <span data-ttu-id="5ab2d-116">`DataSourceID`: Die ID der Datenquelle</span><span class="sxs-lookup"><span data-stu-id="5ab2d-116">`DataSourceID`: The ID of the data source</span></span>
- <span data-ttu-id="5ab2d-117">`SortOrderField`: Die Eigenschaft Sortierungskriterium</span><span class="sxs-lookup"><span data-stu-id="5ab2d-117">`SortOrderField`: The property to sort by</span></span>
- <span data-ttu-id="5ab2d-118">`AllowReorder`: Angibt, ob der Benutzer auf die Listenelemente neu anordnen.</span><span class="sxs-lookup"><span data-stu-id="5ab2d-118">`AllowReorder`: Whether to allow the user to reorder the list elements</span></span>
- <span data-ttu-id="5ab2d-119">`PostBackOnReorder`: Angibt, ob einen Postback erstellen, wenn die Liste neu angeordnet ist</span><span class="sxs-lookup"><span data-stu-id="5ab2d-119">`PostBackOnReorder`: Whether to create a postback whenever the list is rearranged</span></span>

<span data-ttu-id="5ab2d-120">So sieht das entsprechende Markup für das Steuerelement aus:</span><span class="sxs-lookup"><span data-stu-id="5ab2d-120">Here is the appropriate markup for the control:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample2.aspx)]

<span data-ttu-id="5ab2d-121">Innerhalb der `ReorderList` Steuerelement bestimmte Daten aus der Datenquelle kann gebunden werden, mithilfe der `Eval()` Methode:</span><span class="sxs-lookup"><span data-stu-id="5ab2d-121">Within the `ReorderList` control, specific data from the data source may be bound using the `Eval()` method:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample3.aspx)]

<span data-ttu-id="5ab2d-122">An einer beliebigen Stelle auf der Seite wird eine Bezeichnung die Informationen enthalten, bei der letzten Neuanordnen von aufgetreten ist:</span><span class="sxs-lookup"><span data-stu-id="5ab2d-122">At an arbitrary position on the page, a label will hold the information when the last reordering occurred:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample4.aspx)]

<span data-ttu-id="5ab2d-123">Diese Bezeichnung wird Text im serverseitigen Code behandeln das Postback eingetragen:</span><span class="sxs-lookup"><span data-stu-id="5ab2d-123">This label is filled with text in the server-side code, handling the postback:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample5.aspx)]

<span data-ttu-id="5ab2d-124">Schließlich, um die Funktionalität von ASP.NET AJAX und das Steuerelement-Toolkit aktivieren die `ScriptManager` Steuerelement muss auf der Seite platziert werden:</span><span class="sxs-lookup"><span data-stu-id="5ab2d-124">Finally, in order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put on the page:</span></span>

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample6.aspx)]


<span data-ttu-id="5ab2d-125">[![Jede Neuanordnen von löst einen postback](using-postbacks-with-reorderlist-cs/_static/image2.png)](using-postbacks-with-reorderlist-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="5ab2d-125">[![Each reordering triggers a postback](using-postbacks-with-reorderlist-cs/_static/image2.png)](using-postbacks-with-reorderlist-cs/_static/image1.png)</span></span>

<span data-ttu-id="5ab2d-126">Jede Neuanordnen von löst einen Postback ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-postbacks-with-reorderlist-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="5ab2d-126">Each reordering triggers a postback ([Click to view full-size image](using-postbacks-with-reorderlist-cs/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="5ab2d-127">Nächste</span><span class="sxs-lookup"><span data-stu-id="5ab2d-127">Next</span></span>](drag-and-drop-via-reorderlist-cs.md)
