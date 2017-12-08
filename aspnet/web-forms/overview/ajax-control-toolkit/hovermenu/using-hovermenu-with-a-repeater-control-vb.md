---
uid: web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-vb
title: Verwenden HoverMenu mit einem Wiederholungsmodul-Steuerelement (VB) | Microsoft Docs
author: wenz
description: "Das Steuerelement HoverMenu im AJAX-Steuerelement-Toolkit bietet einen einfachen Popup-Effekt: Wenn der Mauszeiger über ein Element bewegt wird, ein Popupfenster angezeigt wird, an einer speziell..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 7f07c112-cd4f-4427-9699-57cfab2791fd
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/hovermenu/using-hovermenu-with-a-repeater-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 77759afe00f341e15ee8e0f52a469d3b7c08ea89
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="using-hovermenu-with-a-repeater-control-vb"></a><span data-ttu-id="426d4-103">Verwenden HoverMenu mit einem Wiederholungsmodul-Steuerelement (VB)</span><span class="sxs-lookup"><span data-stu-id="426d4-103">Using HoverMenu with a Repeater Control (VB)</span></span>
====================
<span data-ttu-id="426d4-104">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="426d4-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="426d4-105">[Herunterladen von Code](http://download.microsoft.com/download/b/0/6/b06fe835-5b8f-4c00-aef8-062c19d75b95/HoverMenu1.vb.zip) oder [PDF herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/hovermenu1VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="426d4-105">[Download Code](http://download.microsoft.com/download/b/0/6/b06fe835-5b8f-4c00-aef8-062c19d75b95/HoverMenu1.vb.zip) or [Download PDF](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/hovermenu1VB.pdf)</span></span>

> <span data-ttu-id="426d4-106">Das Steuerelement HoverMenu im AJAX-Steuerelement-Toolkit bietet einen einfachen Popup-Effekt: Wenn der Mauszeiger über ein Element bewegt wird, ein Popupfenster angezeigt wird, an einer angegebenen Position.</span><span class="sxs-lookup"><span data-stu-id="426d4-106">The HoverMenu control in the AJAX Control Toolkit provides a simple popup effect: When the mouse pointer hovers over an element, a popup appears at a specified position.</span></span> <span data-ttu-id="426d4-107">Es ist auch möglich, dieses Steuerelement in ein Repeater zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="426d4-107">It is also possible to use this control within a repeater.</span></span>


## <a name="overview"></a><span data-ttu-id="426d4-108">Übersicht</span><span class="sxs-lookup"><span data-stu-id="426d4-108">Overview</span></span>

<span data-ttu-id="426d4-109">Die `HoverMenu` Steuerelement im AJAX-Steuerelement-Toolkit stellt eine einfache Popup Auswirkung: Wenn der Mauszeiger über ein Element bewegt wird, ein Popupfenster angezeigt wird, an einer angegebenen Position.</span><span class="sxs-lookup"><span data-stu-id="426d4-109">The `HoverMenu` control in the AJAX Control Toolkit provides a simple popup effect: When the mouse pointer hovers over an element, a popup appears at a specified position.</span></span> <span data-ttu-id="426d4-110">Es ist auch möglich, dieses Steuerelement in ein Repeater zu verwenden.</span><span class="sxs-lookup"><span data-stu-id="426d4-110">It is also possible to use this control within a repeater.</span></span>

## <a name="steps"></a><span data-ttu-id="426d4-111">Schritte</span><span class="sxs-lookup"><span data-stu-id="426d4-111">Steps</span></span>

<span data-ttu-id="426d4-112">Erstens ist eine Datenquelle erforderlich.</span><span class="sxs-lookup"><span data-stu-id="426d4-112">First of all, a data source is required.</span></span> <span data-ttu-id="426d4-113">Dieses Beispiel verwendet die AdventureWorks-Datenbank und die Microsoft SQL Server 2005 Express Edition.</span><span class="sxs-lookup"><span data-stu-id="426d4-113">This sample uses the AdventureWorks database and the Microsoft SQL Server 2005 Express Edition.</span></span> <span data-ttu-id="426d4-114">Die Datenbank ist ein optionaler Teil einer Visual Studio-Installation (einschließlich express Edition) und steht auch als separater Download unter [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="426d4-114">The database is an optional part of a Visual Studio installation (including express edition) and is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="426d4-115">Die AdventureWorks-Datenbank ist Teil der SQL Server 2005 Samples and Sample Databases (download [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang = En](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span><span class="sxs-lookup"><span data-stu-id="426d4-115">The AdventureWorks database is part of the SQL Server 2005 Samples and Sample Databases (download at [https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=e719ecf7-9f46-4312-af89-6ad8702e4e6e&amp;DisplayLang=en)).</span></span> <span data-ttu-id="426d4-116">Die einfachste Möglichkeit zum Einrichten der Datenbank ist die Verwendung der Microsoft SQL Server Management Studio Express ([Https://www.microsoft.com/downloads/details.aspx? FamilyID = c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = En](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)), und fügen die `AdventureWorks.mdf` Datenbankdatei.</span><span class="sxs-lookup"><span data-stu-id="426d4-116">The easiest way to set the database up is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en)) and attach the `AdventureWorks.mdf` database file.</span></span>

<span data-ttu-id="426d4-117">In diesem Beispiel gehen wir davon aus, dass die Instanz von SQL Server 2005 Express Edition aufgerufen wird `SQLEXPRESS` und befindet sich auf dem gleichen Computer wie der Webserver; Dies ist auch die Standardeinstellungen.</span><span class="sxs-lookup"><span data-stu-id="426d4-117">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="426d4-118">Wenn Ihr Setup abweicht, müssen Sie passen Sie die Verbindungsinformationen für die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="426d4-118">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="426d4-119">Um die Funktionalität von ASP.NET AJAX und das Steuerelement-Toolkit aktivieren die `ScriptManager` Steuerelement an einer beliebigen Stelle auf der Seite versetzt werden muss (jedoch innerhalb der `<form>` Element):</span><span class="sxs-lookup"><span data-stu-id="426d4-119">In order to activate the functionality of ASP.NET AJAX and the Control Toolkit, the `ScriptManager` control must be put anywhere on the page (but within the `<form>` element):</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample1.aspx)]

<span data-ttu-id="426d4-120">Fügen Sie dann eine Datenquelle auf der Seite "ein.</span><span class="sxs-lookup"><span data-stu-id="426d4-120">Then, add a data source to the page.</span></span> <span data-ttu-id="426d4-121">Um eine begrenzte Menge von Daten zu verwenden, wählen wir nur die ersten fünf Einträge in der Vendor-Tabelle der AdventureWorks-Datenbank.</span><span class="sxs-lookup"><span data-stu-id="426d4-121">In order to use a limited amount of data, we only select the first five entries in the Vendor table of the AdventureWorks database.</span></span> <span data-ttu-id="426d4-122">Wenn Sie die Visual Studio-Assistent zum Erstellen der Datenquelle verwenden, beachten Sie, dass ein Fehler in der aktuellen Version nicht den Tabellennamen Präfixlänge ist (`Vendor`) mit `Purchasing`.</span><span class="sxs-lookup"><span data-stu-id="426d4-122">If you are using the Visual Studio assistant to create the data source, mind that a bug in the current version does not prefix the table name (`Vendor`) with `Purchasing`.</span></span> <span data-ttu-id="426d4-123">Das folgende Markup zeigt die richtige Syntax:</span><span class="sxs-lookup"><span data-stu-id="426d4-123">The following markup shows the correct syntax:</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample2.aspx)]

<span data-ttu-id="426d4-124">Als Nächstes fügen Sie ein Panel die als modales Popupdialogfeld dient:</span><span class="sxs-lookup"><span data-stu-id="426d4-124">Next, add a panel which serves as the modal popup:</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample3.aspx)]

<span data-ttu-id="426d4-125">Jetzt die `HoverMenuExtender` , kommt es zu.</span><span class="sxs-lookup"><span data-stu-id="426d4-125">Now, the `HoverMenuExtender` comes into play.</span></span> <span data-ttu-id="426d4-126">So, dass jedes Element in der Datenquelle einen eigenen Popup erhält, muss der Extender versetzt werden, innerhalb des wiederholungsmoduls `<ItemTemplate>` Abschnitt.</span><span class="sxs-lookup"><span data-stu-id="426d4-126">So that every element in the data source gets its own popup, the extender must be put within the repeater's `<ItemTemplate>` section.</span></span> <span data-ttu-id="426d4-127">Dies ist das Markup:</span><span class="sxs-lookup"><span data-stu-id="426d4-127">Here is the markup:</span></span>

[!code-aspx[Main](using-hovermenu-with-a-repeater-control-vb/samples/sample4.aspx)]

<span data-ttu-id="426d4-128">Jetzt jedes Element in der Datenquelle ein Popups auf der rechten Seite zeigt (`PopupPosition` Attribut) nach einer Verzögerung von 50 Millisekunden (`PopDelay` Attribut).</span><span class="sxs-lookup"><span data-stu-id="426d4-128">Now every item in the data source displays a popup to the right (`PopupPosition` attribute) after a delay of 50 milliseconds (`PopDelay` attribute).</span></span>


<span data-ttu-id="426d4-129">[![Menü wird neben jedem Element im wiederholungsmodul angezeigt.](using-hovermenu-with-a-repeater-control-vb/_static/image2.png)](using-hovermenu-with-a-repeater-control-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="426d4-129">[![The hover menu appears next to each item in the repeater](using-hovermenu-with-a-repeater-control-vb/_static/image2.png)](using-hovermenu-with-a-repeater-control-vb/_static/image1.png)</span></span>

<span data-ttu-id="426d4-130">Menü angezeigt wird, neben jedem Element im wiederholungsmodul ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-hovermenu-with-a-repeater-control-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="426d4-130">The hover menu appears next to each item in the repeater ([Click to view full-size image](using-hovermenu-with-a-repeater-control-vb/_static/image3.png))</span></span>

>[!div class="step-by-step"]
[<span data-ttu-id="426d4-131">Zurück</span><span class="sxs-lookup"><span data-stu-id="426d4-131">Previous</span></span>](using-hovermenu-with-a-repeater-control-cs.md)
