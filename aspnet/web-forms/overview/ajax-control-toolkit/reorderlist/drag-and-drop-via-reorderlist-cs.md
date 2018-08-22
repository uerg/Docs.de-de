---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-cs
title: Drag & Drop über ReorderList (c#) | Microsoft-Dokumentation
author: wenz
description: Die ReorderList-Steuerelement im AJAX Control Toolkit enthält eine Liste, die durch den Benutzer über Drag & Drop neu angeordnet werden kann. Die Liste der aktuellen Taskreihenfolge sollte...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 6350ee8e-11d6-4aff-b51c-942878014835
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/drag-and-drop-via-reorderlist-cs
msc.type: authoredcontent
ms.openlocfilehash: 15ae6ae60381f3f656f667a97dac72dbb283c80e
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827334"
---
<a name="drag-and-drop-via-reorderlist-c"></a><span data-ttu-id="b9a8e-104">Drag & Drop über ReorderList (c#)</span><span class="sxs-lookup"><span data-stu-id="b9a8e-104">Drag and Drop via ReorderList (C#)</span></span>
====================
<span data-ttu-id="b9a8e-105">durch [Christian Wenz](https://github.com/wenz)</span><span class="sxs-lookup"><span data-stu-id="b9a8e-105">by [Christian Wenz](https://github.com/wenz)</span></span>

<span data-ttu-id="b9a8e-106">[Code herunterladen](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.cs.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5CS.pdf)</span><span class="sxs-lookup"><span data-stu-id="b9a8e-106">[Download Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.cs.zip) or [Download PDF](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5CS.pdf)</span></span>

> <span data-ttu-id="b9a8e-107">Die ReorderList-Steuerelement im AJAX Control Toolkit enthält eine Liste, die durch den Benutzer über Drag & Drop neu angeordnet werden kann.</span><span class="sxs-lookup"><span data-stu-id="b9a8e-107">The ReorderList control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="b9a8e-108">Die Liste der aktuellen Taskreihenfolge muss auf dem Server beibehalten werden.</span><span class="sxs-lookup"><span data-stu-id="b9a8e-108">The current order of the list shall be persisted on the server.</span></span>


## <a name="overview"></a><span data-ttu-id="b9a8e-109">Übersicht</span><span class="sxs-lookup"><span data-stu-id="b9a8e-109">Overview</span></span>

<span data-ttu-id="b9a8e-110">Die `ReorderList` Steuerelement im AJAX Control Toolkit enthält eine Liste, die durch den Benutzer über Drag & Drop neu angeordnet werden kann.</span><span class="sxs-lookup"><span data-stu-id="b9a8e-110">The `ReorderList` control in the AJAX Control Toolkit provides a list that can be reordered by the user via drag and drop.</span></span> <span data-ttu-id="b9a8e-111">Die Liste der aktuellen Taskreihenfolge muss auf dem Server beibehalten werden.</span><span class="sxs-lookup"><span data-stu-id="b9a8e-111">The current order of the list shall be persisted on the server.</span></span>

## <a name="steps"></a><span data-ttu-id="b9a8e-112">Schritte</span><span class="sxs-lookup"><span data-stu-id="b9a8e-112">Steps</span></span>

<span data-ttu-id="b9a8e-113">Die `ReorderList` -Steuerelement unterstützt, Binden von Daten aus einer Datenbank in der Liste.</span><span class="sxs-lookup"><span data-stu-id="b9a8e-113">The `ReorderList` control supports binding data from a database to the list.</span></span> <span data-ttu-id="b9a8e-114">Das beste daran ist, unterstützt aber auch das Schreiben von Änderungen Sequenznummern Listenelements zurück an den Datenspeicher.</span><span class="sxs-lookup"><span data-stu-id="b9a8e-114">Best of all, it also supports writing changes to the order of the list element back to the data store.</span></span>

<span data-ttu-id="b9a8e-115">Dieses Beispiel verwendet die Microsoft SQL Server 2005 Express Edition als Datenspeicher.</span><span class="sxs-lookup"><span data-stu-id="b9a8e-115">This sample uses Microsoft SQL Server 2005 Express Edition as the data store.</span></span> <span data-ttu-id="b9a8e-116">Die Datenbank ist ein optional (und kostenlosen) Teil einer Installation von Visual Studio, einschließlich der express Edition.</span><span class="sxs-lookup"><span data-stu-id="b9a8e-116">The database is an optional (and free) part of a Visual Studio installation, including express edition.</span></span> <span data-ttu-id="b9a8e-117">Es ist auch verfügbar als separater Download unter [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064).</span><span class="sxs-lookup"><span data-stu-id="b9a8e-117">It is also available as a separate download under [https://go.microsoft.com/fwlink/?LinkId=64064](https://go.microsoft.com/fwlink/?LinkId=64064).</span></span> <span data-ttu-id="b9a8e-118">In diesem Beispiel wird angenommen, dass, dass Sie die Instanz von SQL Server 2005 Express Edition aufgerufen wird `SQLEXPRESS` und befindet sich auf dem gleichen Computer wie der Webserver; Dies ist auch der standardeinrichtung.</span><span class="sxs-lookup"><span data-stu-id="b9a8e-118">For this sample, we assume that the instance of the SQL Server 2005 Express Edition is called `SQLEXPRESS` and resides on the same machine as the web server; this is also the default setup.</span></span> <span data-ttu-id="b9a8e-119">Wenn Ihr Setup unterscheidet, müssen Sie die Verbindungsinformationen für die Datenbank anpassen.</span><span class="sxs-lookup"><span data-stu-id="b9a8e-119">If your setup differs, you have to adapt the connection information for the database.</span></span>

<span data-ttu-id="b9a8e-120">Die einfachste Möglichkeit zum Einrichten der Datenbank ist die Verwendung der Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = En](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span><span class="sxs-lookup"><span data-stu-id="b9a8e-120">The easiest way to set up the database is to use the Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ).</span></span> <span data-ttu-id="b9a8e-121">Mit dem Server verbinden, doppelklicken Sie auf `Databases` und eine neue Datenbank erstellen (mit der rechten Maustaste, und wählen Sie `New Database`) namens `Tutorials`.</span><span class="sxs-lookup"><span data-stu-id="b9a8e-121">Connect to the server, double-click on `Databases` and create a new database (right-click and choose `New Database`) called `Tutorials`.</span></span>

<span data-ttu-id="b9a8e-122">In dieser Datenbank, erstellen Sie eine neue Tabelle namens `AJAX` mit den folgenden vier Spalten:</span><span class="sxs-lookup"><span data-stu-id="b9a8e-122">In this database, create a new table called `AJAX` with the following four columns:</span></span>

- <span data-ttu-id="b9a8e-123">`id` (primärer Schlüssel, Integer, Identität, nicht NULL)</span><span class="sxs-lookup"><span data-stu-id="b9a8e-123">`id` (primary key, integer, identity, not NULL)</span></span>
- <span data-ttu-id="b9a8e-124">`char` (char(1), NULL)</span><span class="sxs-lookup"><span data-stu-id="b9a8e-124">`char` (char(1), NULL)</span></span>
- <span data-ttu-id="b9a8e-125">`description` (varchar(50)-Spalte, NULL)</span><span class="sxs-lookup"><span data-stu-id="b9a8e-125">`description` (varchar(50), NULL)</span></span>
- <span data-ttu-id="b9a8e-126">`position` (Int, NULL)</span><span class="sxs-lookup"><span data-stu-id="b9a8e-126">`position` (int, NULL)</span></span>


<span data-ttu-id="b9a8e-127">[![Das Layout der AJAX-Tabelle](drag-and-drop-via-reorderlist-cs/_static/image2.png)](drag-and-drop-via-reorderlist-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="b9a8e-127">[![The layout of the AJAX table](drag-and-drop-via-reorderlist-cs/_static/image2.png)](drag-and-drop-via-reorderlist-cs/_static/image1.png)</span></span>

<span data-ttu-id="b9a8e-128">Das Layout der AJAX-Tabelle ([klicken Sie, um das Bild in voller Größe anzeigen](drag-and-drop-via-reorderlist-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="b9a8e-128">The layout of the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-cs/_static/image3.png))</span></span>


<span data-ttu-id="b9a8e-129">Füllen Sie dann die Tabelle mit ein Paar von Werten.</span><span class="sxs-lookup"><span data-stu-id="b9a8e-129">Next, fill the table with a couple of values.</span></span> <span data-ttu-id="b9a8e-130">Beachten Sie, dass die `position` -Spalte enthält die Sortierreihenfolge der Elemente.</span><span class="sxs-lookup"><span data-stu-id="b9a8e-130">Note that the `position` column holds the sort order of the elements.</span></span>


<span data-ttu-id="b9a8e-131">[![Die ersten Daten in der AJAX-Tabelle](drag-and-drop-via-reorderlist-cs/_static/image5.png)](drag-and-drop-via-reorderlist-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="b9a8e-131">[![The initial data in the AJAX table](drag-and-drop-via-reorderlist-cs/_static/image5.png)](drag-and-drop-via-reorderlist-cs/_static/image4.png)</span></span>

<span data-ttu-id="b9a8e-132">Die ersten Daten in der AJAX-Tabelle ([klicken Sie, um das Bild in voller Größe anzeigen](drag-and-drop-via-reorderlist-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="b9a8e-132">The initial data in the AJAX table ([Click to view full-size image](drag-and-drop-via-reorderlist-cs/_static/image6.png))</span></span>


<span data-ttu-id="b9a8e-133">Der nächste Schritt ist erforderlich, zum Generieren einer `SqlDataSource` Steuerelement für die Kommunikation mit der neuen Datenbank und die Tabelle.</span><span class="sxs-lookup"><span data-stu-id="b9a8e-133">The next step requires to generate an `SqlDataSource` control to communicate with the new database and its table.</span></span> <span data-ttu-id="b9a8e-134">Die Datenquelle muss unterstützen die `SELECT` und `UPDATE` SQL-Befehle.</span><span class="sxs-lookup"><span data-stu-id="b9a8e-134">The data source must support the `SELECT` and `UPDATE` SQL commands.</span></span> <span data-ttu-id="b9a8e-135">Wenn die Reihenfolge der Listenelemente später geändert wird, die `ReorderList` Steuerelement werden automatisch zwei Werte an der Datenquelle sendet `Update` Befehl: die neue Position und die ID des Elements.</span><span class="sxs-lookup"><span data-stu-id="b9a8e-135">When the order of the list elements is later changed, the `ReorderList` control automatically submits two values to the data source's `Update` command: the new position and the ID of the element.</span></span> <span data-ttu-id="b9a8e-136">Aus diesem Grund die Datenquelle muss ein `<UpdateParameters>` Abschnitt für diese beiden Werte:</span><span class="sxs-lookup"><span data-stu-id="b9a8e-136">Therefore, the data source needs an `<UpdateParameters>` section for these two values:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample1.aspx)]

<span data-ttu-id="b9a8e-137">Die `ReorderList` Steuerelement muss die folgenden Attribute festzulegen:</span><span class="sxs-lookup"><span data-stu-id="b9a8e-137">The `ReorderList` control needs to set the following attributes:</span></span>

- <span data-ttu-id="b9a8e-138">`AllowReorder`: Gibt an, ob Elemente der Liste neu angeordnet werden können</span><span class="sxs-lookup"><span data-stu-id="b9a8e-138">`AllowReorder`: Whether the list items may be rearranged</span></span>
- <span data-ttu-id="b9a8e-139">`DataSourceID`: Die ID der Datenquelle</span><span class="sxs-lookup"><span data-stu-id="b9a8e-139">`DataSourceID`: The ID of the data source</span></span>
- <span data-ttu-id="b9a8e-140">`DataKeyField`: Der Name der Primärschlüsselspalte in der Datenquelle</span><span class="sxs-lookup"><span data-stu-id="b9a8e-140">`DataKeyField`: The name of the primary key column in the data source</span></span>
- <span data-ttu-id="b9a8e-141">`SortOrderField`: Die Quellspalte für Daten, die die Sortierreihenfolge für die Listenelemente bereitstellt</span><span class="sxs-lookup"><span data-stu-id="b9a8e-141">`SortOrderField`: The data source column that provides the sort order for the list items</span></span>

<span data-ttu-id="b9a8e-142">In der `<DragHandleTemplate>` und `<ItemTemplate>` Abschnitte, das Layout der Liste angepasst werden kann.</span><span class="sxs-lookup"><span data-stu-id="b9a8e-142">In the `<DragHandleTemplate>` and `<ItemTemplate>` sections, the layout of the list can be fine-tuned.</span></span> <span data-ttu-id="b9a8e-143">Datenbindung ist auch möglich mit der `Eval()` -Methode, wie hier gezeigt:</span><span class="sxs-lookup"><span data-stu-id="b9a8e-143">Also, databinding is possible using the `Eval()` method, as seen here:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample2.aspx)]

<span data-ttu-id="b9a8e-144">Das folgende CSS-Formatinformationen (verwiesen wird, in der `<DragHandleTemplate>` Teil der `ReorderList` Steuerelement) wird sichergestellt, dass sich der Mauszeiger entsprechend ändert, wenn sie über den Ziehpunkt zeigen:</span><span class="sxs-lookup"><span data-stu-id="b9a8e-144">The following CSS style information (referenced in the `<DragHandleTemplate>` section of the `ReorderList` control) makes sure that the mouse pointer changes appropriately when it hovers over the drag handle:</span></span>

[!code-css[Main](drag-and-drop-via-reorderlist-cs/samples/sample3.css)]

<span data-ttu-id="b9a8e-145">Zum Schluss eine `ScriptManager` Steuerelement initialisiert ASP.NET AJAX für die Seite:</span><span class="sxs-lookup"><span data-stu-id="b9a8e-145">Finally, a `ScriptManager` control initializes ASP.NET AJAX for the page:</span></span>

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample4.aspx)]

<span data-ttu-id="b9a8e-146">Führen Sie in diesem Beispiel wird im Browser aus, und ordnen Sie die Listenelemente etwas.</span><span class="sxs-lookup"><span data-stu-id="b9a8e-146">Run this example in the browser and rearrange the list items a bit.</span></span> <span data-ttu-id="b9a8e-147">Anschließend laden Sie die Seite bzw. verfügen Sie einen Blick auf die Datenbank.</span><span class="sxs-lookup"><span data-stu-id="b9a8e-147">Then, reload the page and/or have a look at the database.</span></span> <span data-ttu-id="b9a8e-148">Die geänderten Positionen noch gültig und werden auch übernommen, nach den Werten in der `position` -Spalte in die Datenbank ohne Code, nur mithilfe von Markup.</span><span class="sxs-lookup"><span data-stu-id="b9a8e-148">The altered positions have been maintained and are also reflected by the values in the `position` column in the database and that all without any code, just by using markup.</span></span>


<span data-ttu-id="b9a8e-149">[![Das Datenelement in der Datenbank ändert sich gemäß der neuen Liste Reihenfolge](drag-and-drop-via-reorderlist-cs/_static/image8.png)](drag-and-drop-via-reorderlist-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="b9a8e-149">[![The data in the database changes according to the new list item order](drag-and-drop-via-reorderlist-cs/_static/image8.png)](drag-and-drop-via-reorderlist-cs/_static/image7.png)</span></span>

<span data-ttu-id="b9a8e-150">Das Datenelement in der Datenbank ändert sich gemäß der neuen Liste Reihenfolge ([klicken Sie, um das Bild in voller Größe anzeigen](drag-and-drop-via-reorderlist-cs/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="b9a8e-150">The data in the database changes according to the new list item order ([Click to view full-size image](drag-and-drop-via-reorderlist-cs/_static/image9.png))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="b9a8e-151">[Zurück](using-postbacks-with-reorderlist-cs.md)
> [Weiter](using-postbacks-with-reorderlist-vb.md)</span><span class="sxs-lookup"><span data-stu-id="b9a8e-151">[Previous](using-postbacks-with-reorderlist-cs.md)
[Next](using-postbacks-with-reorderlist-vb.md)</span></span>
