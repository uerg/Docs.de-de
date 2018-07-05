---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
title: Verarbeiten von Postbacks über ein Popupsteuerelement mit einem UpdatePanel-Steuerelement (c#) | Microsoft-Dokumentation
author: wenz
description: Der PopupControl Extender im AJAX Control Toolkit bietet eine einfache Möglichkeit, ein Popup auslösen, wenn ein anderes Steuerelement aktiviert ist. Besondere Sorgfalt verfügt, die ausgeführt werden...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 1f68f59d-9c1e-4cf3-b304-c13ae6b7203e
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: 9fdf2821e005bfcb691134bb6994c01fa72b7abd
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37815381"
---
<a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-c"></a><span data-ttu-id="48a32-104">Verarbeiten von Postbacks über ein Popupsteuerelement mit einem UpdatePanel-Steuerelement (c#)</span><span class="sxs-lookup"><span data-stu-id="48a32-104">Handling Postbacks from A Popup Control With an UpdatePanel (C#)</span></span>
====================
<span data-ttu-id="48a32-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="48a32-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="48a32-106">[Code herunterladen](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.cs.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="48a32-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2CS.pdf)</span></span>

> <span data-ttu-id="48a32-107">Der PopupControl Extender im AJAX Control Toolkit bietet eine einfache Möglichkeit, ein Popup auslösen, wenn ein anderes Steuerelement aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="48a32-107">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="48a32-108">Besondere Sorgfalt muss ausgeführt werden, wenn ein Postback solche ein Popup erfolgt.</span><span class="sxs-lookup"><span data-stu-id="48a32-108">Special care has to be taken when a postback occurs within such a popup.</span></span>


## <a name="overview"></a><span data-ttu-id="48a32-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="48a32-109">Overview</span></span>

<span data-ttu-id="48a32-110">Der PopupControl Extender im AJAX Control Toolkit bietet eine einfache Möglichkeit, ein Popup auslösen, wenn ein anderes Steuerelement aktiviert ist.</span><span class="sxs-lookup"><span data-stu-id="48a32-110">The PopupControl extender in the AJAX Control Toolkit offers an easy way to trigger a popup when any other control is activated.</span></span> <span data-ttu-id="48a32-111">Besondere Sorgfalt muss ausgeführt werden, wenn ein Postback solche ein Popup erfolgt.</span><span class="sxs-lookup"><span data-stu-id="48a32-111">Special care has to be taken when a postback occurs within such a popup.</span></span>

## <a name="steps"></a><span data-ttu-id="48a32-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="48a32-112">Steps</span></span>

<span data-ttu-id="48a32-113">Bei Verwendung einer `PopupControl` mit einem Postback, ein `UpdatePanel` können verhindern, dass die seitenaktualisierung, die durch das Postback verursacht.</span><span class="sxs-lookup"><span data-stu-id="48a32-113">When using a `PopupControl` with a postback, an `UpdatePanel` can prevent the page refresh caused by the postback.</span></span> <span data-ttu-id="48a32-114">Das folgende Markup definiert ein paar wichtige Elemente:</span><span class="sxs-lookup"><span data-stu-id="48a32-114">The following markup defines a couple of important elements:</span></span>

- <span data-ttu-id="48a32-115">Ein `ScriptManager` steuern, das ASP.NET AJAX Control Toolkit funktioniert</span><span class="sxs-lookup"><span data-stu-id="48a32-115">A `ScriptManager` control so that the ASP.NET AJAX Control Toolkit works</span></span>
- <span data-ttu-id="48a32-116">Zwei `TextBox` steuert, welche sowohl ein Popup ausgelöst werden</span><span class="sxs-lookup"><span data-stu-id="48a32-116">Two `TextBox` controls which will both trigger a popup</span></span>
- <span data-ttu-id="48a32-117">Ein `Panel` -Steuerelement, das als das Popup verwendet wird</span><span class="sxs-lookup"><span data-stu-id="48a32-117">A `Panel` control that will serve as the popup</span></span>
- <span data-ttu-id="48a32-118">Innerhalb des Bereichs eine `Calendar` eingebettetes Steuerelement ist in einer `UpdatePanel` Steuerelement</span><span class="sxs-lookup"><span data-stu-id="48a32-118">Within the panel, a `Calendar` control is embedded within an `UpdatePanel` control</span></span>
- <span data-ttu-id="48a32-119">Zwei `PopupControlExtender` Steuerelemente, die den Inhalt der Textfelder im Bereich zuweisen</span><span class="sxs-lookup"><span data-stu-id="48a32-119">Two `PopupControlExtender` controls that assign the panel to the text boxes</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample1.aspx)]

<span data-ttu-id="48a32-120">Beachten Sie, dass die `OnSelectionChanged` Attribut der `Calendar` -Steuerelement so eingestellt ist.</span><span class="sxs-lookup"><span data-stu-id="48a32-120">Note that the `OnSelectionChanged` attribute of the `Calendar` control is set.</span></span> <span data-ttu-id="48a32-121">Daher ein Postback tritt auf, wenn der Benutzer ein Datum innerhalb des Kalenders auswählt, und die serverseitige Methode `c1_SelectionChanged()` ausgeführt wird.</span><span class="sxs-lookup"><span data-stu-id="48a32-121">So when the user selects a date within the calendar, a postback occurs and the server-side method `c1_SelectionChanged()` is executed.</span></span> <span data-ttu-id="48a32-122">In dieser Methode muss das aktuelle Datum abgerufen und in das Textfeld zurückgeschrieben werden.</span><span class="sxs-lookup"><span data-stu-id="48a32-122">Within that method, the current date must be retrieved and written back to the textbox.</span></span>

<span data-ttu-id="48a32-123">Die Syntax dafür lautet wie folgt: zunächst einen Proxy-Objekt für die `PopupControlExtender` auf der Seite generiert werden muss.</span><span class="sxs-lookup"><span data-stu-id="48a32-123">The syntax for that is as follows: First of all, a proxy object for the `PopupControlExtender` on the page must be generated.</span></span> <span data-ttu-id="48a32-124">Das ASP.NET AJAX Control Toolkit bietet die `GetProxyForCurrentPopup()` Methode.</span><span class="sxs-lookup"><span data-stu-id="48a32-124">The ASP.NET AJAX Control Toolkit offers the `GetProxyForCurrentPopup()` method.</span></span> <span data-ttu-id="48a32-125">Das Objekt, das Beenden dieser Methode unterstützt die `Commit()` Methode, der einen Wert an das Steuerelement gesendet, der das Popup (nicht das Steuerelement, das den Methodenaufruf ausgelöst!) ausgelöst.</span><span class="sxs-lookup"><span data-stu-id="48a32-125">The object this method returns supports the `Commit()` method which sends a value back to the control that triggered the popup (not the control that triggered the method call!).</span></span> <span data-ttu-id="48a32-126">Der folgende Code stellt das ausgewählte Datum als Argument für die `Commit()` -Methode, sodass des Codes, die das ausgewählte Datum in das Textfeld schreiben:</span><span class="sxs-lookup"><span data-stu-id="48a32-126">The following code provides the selected date as the argument for the `Commit()` method, causing the code to write the selected date back to the text box:</span></span>

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample2.aspx)]

<span data-ttu-id="48a32-127">Jetzt, wenn Sie in einem Kalenderdatum klicken Sie auf das ausgewählte Datum angezeigt wird, in der zugeordneten Textfeld ein Datumsauswahl-Steuerelement zu erstellen, die derzeit auf vielen Websites finden.</span><span class="sxs-lookup"><span data-stu-id="48a32-127">Now whenever you click on a calendar date, the selected date appears in the associated text box, creating a date picker control that can currently be found on many websites.</span></span>


<span data-ttu-id="48a32-128">[![Der Kalender wird angezeigt, wenn der Benutzer in das Textfeld klickt.](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="48a32-128">[![The Calendar appears when the user clicks into the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image1.png)</span></span>

<span data-ttu-id="48a32-129">Der Kalender wird angezeigt, wenn der Benutzer in das Textfeld klickt ([klicken Sie, um das Bild in voller Größe anzeigen](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="48a32-129">The Calendar appears when the user clicks into the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image3.png))</span></span>


<span data-ttu-id="48a32-130">[![Durch Klicken auf ein Datum in das Textfeld eingefügt](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="48a32-130">[![Clicking on a date puts it in the textbox](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image4.png)</span></span>

<span data-ttu-id="48a32-131">Durch Klicken auf ein Datum in das Textfeld eingefügt ([klicken Sie, um das Bild in voller Größe anzeigen](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="48a32-131">Clicking on a date puts it in the textbox ([Click to view full-size image](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image6.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="48a32-132">[Zurück](using-multiple-popup-controls-cs.md)
> [Weiter](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)</span><span class="sxs-lookup"><span data-stu-id="48a32-132">[Previous](using-multiple-popup-controls-cs.md)
[Next](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)</span></span>
