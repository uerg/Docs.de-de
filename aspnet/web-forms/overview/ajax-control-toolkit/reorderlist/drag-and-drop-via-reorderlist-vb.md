---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-vb
title: Drag & Drop über ReorderList (VB) | Microsoft Docs
author: wenz
description: /Data-Access/Tutorials/Master-Detail-Using-a-bulleted-List-of-Master-Records-with-a-Details-DataList-VB
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 848e6bcf-4c3f-4d14-974d-e45b9444ab79
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-vb
msc.type: authoredcontent
ms.openlocfilehash: 99f47b969dc75efeec8485254d311c93dc0b5d35
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="drag-and-drop-via-reorderlist-vb"></a><span data-ttu-id="8d9b0-103">Drag & Drop über ReorderList (VB)</span><span class="sxs-lookup"><span data-stu-id="8d9b0-103">Drag and Drop via ReorderList (VB)</span></span>
====================
<span data-ttu-id="8d9b0-104">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="8d9b0-104">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="8d9b0-105">[Herunterladen von Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.vb.zip) oder [PDF herunterladen](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5VB.pdf)</span><span class="sxs-lookup"><span data-stu-id="8d9b0-105">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.vb.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5VB.pdf)</span></span>

> <span data-ttu-id="8d9b0-106">Das Steuerelement ReorderList im AJAX-Steuerelement-Toolkit bietet eine Liste, die vom Benutzer per Drag & Drop neu angeordnet werden kann.</span><span class="sxs-lookup"><span data-stu-id="8d9b0-106">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="8d9b0-107">Die aktuelle Bestellung in der Liste muss auf dem Server beibehalten werden.</span><span class="sxs-lookup"><span data-stu-id="8d9b0-107">The current order of the list shall be persisted on the server.</span></span>


## <a name="overview"></a><span data-ttu-id="8d9b0-108">Übersicht</span><span class="sxs-lookup"><span data-stu-id="8d9b0-108">Overview</span></span>

<span data-ttu-id="8d9b0-109">Die `ReorderList` Steuerelement im AJAX-Steuerelement-Toolkit enthält eine Liste, die vom Benutzer per Drag & Drop neu angeordnet werden kann.</span><span class="sxs-lookup"><span data-stu-id="8d9b0-109">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="8d9b0-110">Die aktuelle Bestellung in der Liste muss auf dem Server beibehalten werden.</span><span class="sxs-lookup"><span data-stu-id="8d9b0-110">The current order of the list shall be persisted on the server.</span></span>

## <a name="steps"></a><span data-ttu-id="8d9b0-111">Schritte</span><span class="sxs-lookup"><span data-stu-id="8d9b0-111">Steps</span></span>

<span data-ttu-id="8d9b0-112">Die `ReorderList` Steuerelement unterstützt Binden von Daten aus einer Datenbank in der Liste.</span><span class="sxs-lookup"><span data-stu-id="8d9b0-112">The `ReorderList` control supports binding data from a database to the list.</span></span> <span data-ttu-id="8d9b0-113">Beste daran ist, wird auch das Schreiben von Änderungen Sequenznummern das Listenelement zurück an den Datenspeicher unterstützt.</span><span class="sxs-lookup"><span data-stu-id="8d9b0-113">Best of all, it also supports writing changes to the order of the list element back to the data store.</span></span>

<span data-ttu-id="8d9b0-114">Dieses Beispiel verwendet Microsoft SQL Server 2005 Express Edition als Datenspeicher.</span><span class="sxs-lookup"><span data-stu-id="8d9b0-114">This sample uses Microsoft SQL Server 2005 Express Edition as the data store.</span></span> <span data-ttu-id="8d9b0-115">Die Datenbank ist ein optional (und kostenlosen) Teil einer Installation von Visual Studio, einschließlich der express Edition.</span><span class="sxs-lookup"><span data-stu-id="8d9b0-115">The database is an optional (and free) part of a Visual Studio installation, including express edition.</span></span> <span data-ttu-id="8d9b0-116">Es ist auch verfügbar als separater Download unter [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="8d9b0-116">It is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="8d9b0-117">In diesem Beispiel gehen wir davon aus, dass die Instanz von SQL Server 2005 Express Edition aufgerufen wird `SQLEXPRESS` und befindet sich auf dem gleichen Computer wie der Webserver; Dies ist auch die Standardeinstellungen.</span><span class="sxs-lookup"><span data-stu-id="8d9b0-117">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="8d9b0-118">Wenn Ihr Setup abweicht, müssen Sie passen Sie die Verbindungsinformationen für die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="8d9b0-118">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="8d9b0-119">Die einfachste Möglichkeit zum Einrichten der Datenbank ist die Verwendung der Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = En](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span><span class="sxs-lookup"><span data-stu-id="8d9b0-119">The easiest way to set up the database is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span></span> <span data-ttu-id="8d9b0-120">Herstellen einer Verbindung mit dem Server, doppelklicken Sie auf `Databases` und eine neue Datenbank erstellen (mit der rechten Maustaste, und wählen Sie `New Database`) aufgerufen `Tutorials`.</span><span class="sxs-lookup"><span data-stu-id="8d9b0-120">Connect to the server, double-click on `Databases` and create a new database (right-click and choose `New Database`) called `Tutorials`.</span></span>

<span data-ttu-id="8d9b0-121">Erstellen Sie in dieser Datenbank eine neue Tabelle namens `AJAX` mit den folgenden vier Spalten:</span><span class="sxs-lookup"><span data-stu-id="8d9b0-121">In this database, create a new table called `AJAX` with the following four columns:</span></span>

- <span data-ttu-id="8d9b0-122">`id` (primary Key, Integer, Identität, not NULL)</span><span class="sxs-lookup"><span data-stu-id="8d9b0-122">`id` (primary key, integer, identity, not NULL)</span></span>
- <span data-ttu-id="8d9b0-123">`char` (char(1), NULL)</span><span class="sxs-lookup"><span data-stu-id="8d9b0-123">`char` (char(1), NULL)</span></span>
- <span data-ttu-id="8d9b0-124">`description` (varchar(50)-Spalte, NULL)</span><span class="sxs-lookup"><span data-stu-id="8d9b0-124">`description` (varchar(50), NULL)</span></span>
- <span data-ttu-id="8d9b0-125">`position` (int, NULL)</span><span class="sxs-lookup"><span data-stu-id="8d9b0-125">`position` (int, NULL)</span></span>


<span data-ttu-id="8d9b0-126">[![Das Layout der AJAX-Tabelle](drag-and-drop-via-reorderlist-vb/_static/image2.png)](drag-and-drop-via-reorderlist-vb/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="8d9b0-126">[![The layout of the AJAX table](drag-and-drop-via-reorderlist-vb/_static/image2.png)](drag-and-drop-via-reorderlist-vb/_static/image1.png)</span></span>

<span data-ttu-id="8d9b0-127">Das Layout der AJAX-Tabelle ([klicken Sie hier, um das Bild in voller Größe angezeigt](drag-and-drop-via-reorderlist-vb/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="8d9b0-127">The layout of the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image3.png))</span></span>


<span data-ttu-id="8d9b0-128">Als Nächstes werden füllen Sie die Tabelle über ein Paar von Werten.</span><span class="sxs-lookup"><span data-stu-id="8d9b0-128">Next, fill the table with a couple of values.</span></span> <span data-ttu-id="8d9b0-129">Beachten Sie, dass die `position` Spalte enthält die Sortierreihenfolge der Elemente.</span><span class="sxs-lookup"><span data-stu-id="8d9b0-129">Note that the `position` column holds the sort order of the elements.</span></span>


<span data-ttu-id="8d9b0-130">[![Die ursprünglichen Daten in der AJAX-Tabelle](drag-and-drop-via-reorderlist-vb/_static/image5.png)](drag-and-drop-via-reorderlist-vb/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="8d9b0-130">[![The initial data in the AJAX table](drag-and-drop-via-reorderlist-vb/_static/image5.png)](drag-and-drop-via-reorderlist-vb/_static/image4.png)</span></span>

<span data-ttu-id="8d9b0-131">Die ursprünglichen Daten in der AJAX-Tabelle ([klicken Sie hier, um das Bild in voller Größe angezeigt](drag-and-drop-via-reorderlist-vb/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="8d9b0-131">The initial data in the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image6.png))</span></span>


<span data-ttu-id="8d9b0-132">Der nächste Schritt ist erforderlich, zum Generieren einer `SqlDataSource` Steuerelement für die Kommunikation mit der neuen Datenbank und die Tabelle.</span><span class="sxs-lookup"><span data-stu-id="8d9b0-132">The next step requires to generate an `SqlDataSource` control to communicate with the new database and its table.</span></span> <span data-ttu-id="8d9b0-133">Die Datenquelle muss unterstützen die `SELECT` und `UPDATE` SQL-Befehle.</span><span class="sxs-lookup"><span data-stu-id="8d9b0-133">The data source must support the `SELECT` and `UPDATE` SQL commands.</span></span> <span data-ttu-id="8d9b0-134">Wenn die Reihenfolge der Listenelemente später geändert wird, die `ReorderList` Steuerelement sendet automatisch zwei Werte an der Datenquelle `Update` Befehl: die neue Position und die ID des Elements.</span><span class="sxs-lookup"><span data-stu-id="8d9b0-134">When the order of the list elements is later changed, the `ReorderList` control automatically submits two values to the data source's `Update` command: the new position and the ID of the element.</span></span> <span data-ttu-id="8d9b0-135">Aus diesem Grund die Datenquelle muss ein `<UpdateParameters>` Abschnitt, um diese beiden Werte:</span><span class="sxs-lookup"><span data-stu-id="8d9b0-135">Therefore, the data source needs an `<UpdateParameters>` section for these two values:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample1.aspx)]

<span data-ttu-id="8d9b0-136">Die `ReorderList` Steuerelement muss die folgenden Attribute festzulegen:</span><span class="sxs-lookup"><span data-stu-id="8d9b0-136">The `ReorderList` control needs to set the following attributes:</span></span>

- <span data-ttu-id="8d9b0-137">`AllowReorder`: Gibt an, ob die Elemente der Liste neu angeordnet werden können</span><span class="sxs-lookup"><span data-stu-id="8d9b0-137">`AllowReorder`: Whether the list items may be rearranged</span></span>
- <span data-ttu-id="8d9b0-138">`DataSourceID`: Die ID der Datenquelle</span><span class="sxs-lookup"><span data-stu-id="8d9b0-138">`DataSourceID`: The ID of the data source</span></span>
- <span data-ttu-id="8d9b0-139">`DataKeyField`: Der Name der Primärschlüsselspalte in der Datenquelle</span><span class="sxs-lookup"><span data-stu-id="8d9b0-139">`DataKeyField`: The name of the primary key column in the data source</span></span>
- <span data-ttu-id="8d9b0-140">`SortOrderField`: Die Datenquellenspalte, die die Sortierreihenfolge für die Listenelemente bereitstellt.</span><span class="sxs-lookup"><span data-stu-id="8d9b0-140">`SortOrderField`: The data source column that provides the sort order for the list items</span></span>

<span data-ttu-id="8d9b0-141">In der `<DragHandleTemplate>` und `<ItemTemplate>` Abschnitte, das Layout der Liste kann optimiert werden.</span><span class="sxs-lookup"><span data-stu-id="8d9b0-141">In the `<DragHandleTemplate>` and `<ItemTemplate>` sections, the layout of the list can be fine-tuned.</span></span> <span data-ttu-id="8d9b0-142">Datenbindung ist außerdem möglich mit der `Eval()` -Methode, wie hier zu sehen:</span><span class="sxs-lookup"><span data-stu-id="8d9b0-142">Also, databinding is possible using the `Eval()` method, as seen here:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample2.aspx)]

<span data-ttu-id="8d9b0-143">Das folgende CSS-Formatinformationen (verwiesen wird, in der `<DragHandleTemplate>` Teil der `ReorderList` Steuerelement) wird sichergestellt, dass der Mauszeiger die Form entsprechend geändert wird, wenn er über dem Ziehpunkt bewegt wird:</span><span class="sxs-lookup"><span data-stu-id="8d9b0-143">The following CSS style information (referenced in the `<DragHandleTemplate>` section of the `ReorderList` control) makes sure that the mouse pointer changes appropriately when it hovers over the drag handle:</span></span>

[!code-css[Main](drag-and-drop-via-reorderlist-vb/samples/sample3.css)]

<span data-ttu-id="8d9b0-144">Schließlich ein `ScriptManager` Steuerelement initialisiert ASP.NET AJAX für die Seite:</span><span class="sxs-lookup"><span data-stu-id="8d9b0-144">Finally, a `ScriptManager` control initializes ASP.NET AJAX for the page:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample4.aspx)]

<span data-ttu-id="8d9b0-145">Führen Sie in diesem Beispiel wird im Browser, und ordnen Sie die Listenelemente etwas.</span><span class="sxs-lookup"><span data-stu-id="8d9b0-145">Run this example in the browser and rearrange the list items a bit.</span></span> <span data-ttu-id="8d9b0-146">Klicken Sie dann die Seite neu zu laden, und/oder einen Blick auf die Datenbank verfügen.</span><span class="sxs-lookup"><span data-stu-id="8d9b0-146">Then, reload the page and/or have a look at the database.</span></span> <span data-ttu-id="8d9b0-147">Die geänderten Positionen gewartet wurden und auch nach den Werten in wiedergegeben werden die `position` Spalte in der Datenbank und, ohne Code, nur mithilfe von Markup.</span><span class="sxs-lookup"><span data-stu-id="8d9b0-147">The altered positions have been maintained and are also reflected by the values in the `position` column in the database and that all without any code, just by using markup.</span></span>


<span data-ttu-id="8d9b0-148">[![Die Daten in der Datenbank ändert sich entsprechend der Reihenfolge der neuen Liste-Element](drag-and-drop-via-reorderlist-vb/_static/image8.png)](drag-and-drop-via-reorderlist-vb/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="8d9b0-148">[![The data in the database changes according to the new list item order](drag-and-drop-via-reorderlist-vb/_static/image8.png)](drag-and-drop-via-reorderlist-vb/_static/image7.png)</span></span>

<span data-ttu-id="8d9b0-149">Das Datenelement in der Datenbank geändert wird, gemäß der neuen Liste Reihenfolge ([klicken Sie hier, um das Bild in voller Größe angezeigt](drag-and-drop-via-reorderlist-vb/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="8d9b0-149">The data in the database changes according to the new list item order ([Click to view full-size image](drag-and-drop-via-reorderlist-vb/_static/image9.png))</span></span>

> [!div class="step-by-step"]
> [<span data-ttu-id="8d9b0-150">Vorherige</span><span class="sxs-lookup"><span data-stu-id="8d9b0-150">Previous</span></span>](using-postbacks-with-reorderlist-vb.md)
