---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
title: Behandlung von Postbacks aus einem Popupsteuerelement mit UpdatePanel (c#) | Microsoft Docs
author: wenz
description: Der Extender PopupControl im AJAX Control Toolkit bietet eine einfache Möglichkeit, um ein Popup auszulösen, wenn es sich bei jedem anderen Steuerelement aktiviert wird. Besondere Sorgfalt hat auszuführende...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 1f68f59d-9c1e-4cf3-b304-c13ae6b7203e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: abedb5247f710b02752651a7bfb011ab63d32844
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879633"
---
<a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-c"></a><span data-ttu-id="723bc-104">Behandlung von Postbacks aus einem Popupsteuerelement mit UpdatePanel (c#)</span><span class="sxs-lookup"><span data-stu-id="723bc-104">Handling Postbacks from A Popup Control With an UpdatePanel (C#)</span></span>
====================
<span data-ttu-id="723bc-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="723bc-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="723bc-106">[Herunterladen von Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.cs.zip) oder [PDF herunterladen](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="723bc-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2CS.pdf)</span></span>

> <span data-ttu-id="723bc-107">Der Extender PopupControl im AJAX Control Toolkit bietet eine einfache Möglichkeit, um ein Popup auszulösen, wenn es sich bei jedem anderen Steuerelement aktiviert wird.</span><span class="sxs-lookup"><span data-stu-id="723bc-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="723bc-108">Besondere Sorgfalt muss ausgeführt werden, wenn ein Postback solche ein Popup erfolgt.</span><span class="sxs-lookup"><span data-stu-id="723bc-108">Special care has to be taken when a postback occurs within such a popup.</span></span>


## <a name="overview"></a><span data-ttu-id="723bc-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="723bc-109">Overview</span></span>

<span data-ttu-id="723bc-110">Der Extender PopupControl im AJAX Control Toolkit bietet eine einfache Möglichkeit, um ein Popup auszulösen, wenn es sich bei jedem anderen Steuerelement aktiviert wird.</span><span class="sxs-lookup"><span data-stu-id="723bc-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="723bc-111">Besondere Sorgfalt muss ausgeführt werden, wenn ein Postback solche ein Popup erfolgt.</span><span class="sxs-lookup"><span data-stu-id="723bc-111">Special care has to be taken when a postback occurs within such a popup.</span></span>

## <a name="steps"></a><span data-ttu-id="723bc-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="723bc-112">Steps</span></span>

<span data-ttu-id="723bc-113">Bei Verwendung einer `PopupControl` mit einem Postback ein `UpdatePanel` können verhindern, dass die Seite-Aktualisierung, die durch das Postback verursacht.</span><span class="sxs-lookup"><span data-stu-id="723bc-113">When using a `PopupControl` with a postback, an `UpdatePanel` can prevent the page refresh caused by the postback.</span></span> <span data-ttu-id="723bc-114">Das folgende Markup definiert eine Reihe von wichtigen Elemente:</span><span class="sxs-lookup"><span data-stu-id="723bc-114">The following markup defines a couple of important elements:</span></span>

- <span data-ttu-id="723bc-115">Ein `ScriptManager` steuern, sodass das ASP.NET AJAX-Steuerelement-Toolkit funktioniert</span><span class="sxs-lookup"><span data-stu-id="723bc-115">A `ScriptManager` control so that the ASP.NET AJAX Control Toolkit works</span></span>
- <span data-ttu-id="723bc-116">Zwei `TextBox` steuert, welche sowohl ein Popup ausgelöst werden</span><span class="sxs-lookup"><span data-stu-id="723bc-116">Two `TextBox` controls which will both trigger a popup</span></span>
- <span data-ttu-id="723bc-117">Ein `Panel` Steuerelement, das als das Popup fungieren soll</span><span class="sxs-lookup"><span data-stu-id="723bc-117">A `Panel` control that will serve as the popup</span></span>
- <span data-ttu-id="723bc-118">Innerhalb des Bereichs einer `Calendar` Steuerelement eingebettet ist ein `UpdatePanel` Steuerelement</span><span class="sxs-lookup"><span data-stu-id="723bc-118">Within the panel, a `Calendar` control is embedded within an `UpdatePanel` control</span></span>
- <span data-ttu-id="723bc-119">Zwei `PopupControlExtender` Steuerelemente, die im Bereich der Textfelder zuweisen</span><span class="sxs-lookup"><span data-stu-id="723bc-119">Two `PopupControlExtender` controls that assign the panel to the text boxes</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample1.aspx)]

<span data-ttu-id="723bc-120">Beachten Sie, dass die `OnSelectionChanged` Attribut von der `Calendar` Steuerelement festgelegt ist.</span><span class="sxs-lookup"><span data-stu-id="723bc-120">Note that the `OnSelectionChanged` attribute of the `Calendar` control is set.</span></span> <span data-ttu-id="723bc-121">Damit ein Postback tritt auf, wenn der Benutzer ein Datum im Kalender auswählt, und die serverseitige Methode `c1_SelectionChanged()` ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="723bc-121">So when the user selects a date within the calendar, a postback occurs and the server-side method `c1_SelectionChanged()` is executed.</span></span> <span data-ttu-id="723bc-122">Innerhalb dieser Methode muss das aktuelle Datum abgerufen und zurück in das Textfeld geschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="723bc-122">Within that method, the current date must be retrieved and written back to the textbox.</span></span>

<span data-ttu-id="723bc-123">Die Syntax lautet wie folgt: Erstens einen Proxy-Objekt für die `PopupControlExtender` auf der Seite generiert werden muss.</span><span class="sxs-lookup"><span data-stu-id="723bc-123">The syntax for that is as follows: First of all, a proxy object for the `PopupControlExtender` on the page must be generated.</span></span> <span data-ttu-id="723bc-124">Das ASP.NET AJAX-Steuerelement-Toolkit bietet die `GetProxyForCurrentPopup()` Methode.</span><span class="sxs-lookup"><span data-stu-id="723bc-124">The ASP.NET AJAX Control Toolkit offers the `GetProxyForCurrentPopup()` method.</span></span> <span data-ttu-id="723bc-125">Das Objekt, diese Methode gibt, unterstützt die `Commit()` Methode einen Wert an das Steuerelement gesendet wird, die das Popup (nicht das Steuerelement, das dem Aufruf der Methode ausgelöst!) ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="723bc-125">The object this method returns supports the `Commit()` method which sends a value back to the control that triggered the popup (not the control that triggered the method call!).</span></span> <span data-ttu-id="723bc-126">Der folgende Code stellt das ausgewählte Datum als Argument für die `Commit()` -Methode, sodass des Codes zum Zurückschreiben von des ausgewählten Datums in das Textfeld:</span><span class="sxs-lookup"><span data-stu-id="723bc-126">The following code provides the selected date as the argument for the `Commit()` method, causing the code to write the selected date back to the text box:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample2.aspx)]

<span data-ttu-id="723bc-127">Jetzt immer an einem Kalenderdatum klicken Sie auf das ausgewählte Datum angezeigt wird, klicken Sie im zugehörigen Textfeld kann ein Datumsauswahl-Steuerelement erstellen, die derzeit auf vielen Websites gefunden werden.</span><span class="sxs-lookup"><span data-stu-id="723bc-127">Now whenever you click on a calendar date, the selected date appears in the associated text box, creating a date picker control that can currently be found on many websites.</span></span>


<span data-ttu-id="723bc-128">[![Der Kalender wird angezeigt, wenn der Benutzer in das Textfeld klickt](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="723bc-128">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image1.png)</span></span>

<span data-ttu-id="723bc-129">Der Kalender wird angezeigt, wenn der Benutzer in das Textfeld klickt ([klicken Sie hier, um das Bild in voller Größe angezeigt](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="723bc-129">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image3.png))</span></span>


<span data-ttu-id="723bc-130">[![Durch Klicken auf ein Datum in das Textfeld eingefügt](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="723bc-130">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image4.png)</span></span>

<span data-ttu-id="723bc-131">Durch Klicken auf ein Datum in das Textfeld eingefügt ([klicken Sie hier, um das Bild in voller Größe angezeigt](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="723bc-131">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="723bc-132">[Zurück](using-multiple-popup-controls-cs.md)
> [Weiter](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)</span><span class="sxs-lookup"><span data-stu-id="723bc-132">[Previous](using-multiple-popup-controls-cs.md)
[Next](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)</span></span>
