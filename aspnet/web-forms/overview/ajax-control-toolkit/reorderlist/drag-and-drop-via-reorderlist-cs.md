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
<a name="drag-and-drop-via-reorderlist-c"></a>Drag & Drop über ReorderList (c#)
====================
durch [Christian Wenz](https://github.com/wenz)

[Code herunterladen](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.cs.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5CS.pdf)

> Die ReorderList-Steuerelement im AJAX Control Toolkit enthält eine Liste, die durch den Benutzer über Drag & Drop neu angeordnet werden kann. Die Liste der aktuellen Taskreihenfolge muss auf dem Server beibehalten werden.


## <a name="overview"></a>Übersicht

Die `ReorderList` Steuerelement im AJAX Control Toolkit enthält eine Liste, die durch den Benutzer über Drag & Drop neu angeordnet werden kann. Die Liste der aktuellen Taskreihenfolge muss auf dem Server beibehalten werden.

## <a name="steps"></a>Schritte

Die `ReorderList` -Steuerelement unterstützt, Binden von Daten aus einer Datenbank in der Liste. Das beste daran ist, unterstützt aber auch das Schreiben von Änderungen Sequenznummern Listenelements zurück an den Datenspeicher.

Dieses Beispiel verwendet die Microsoft SQL Server 2005 Express Edition als Datenspeicher. Die Datenbank ist ein optional (und kostenlosen) Teil einer Installation von Visual Studio, einschließlich der express Edition. Es ist auch verfügbar als separater Download unter [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). In diesem Beispiel wird angenommen, dass, dass Sie die Instanz von SQL Server 2005 Express Edition aufgerufen wird `SQLEXPRESS` und befindet sich auf dem gleichen Computer wie der Webserver; Dies ist auch der standardeinrichtung. Wenn Ihr Setup unterscheidet, müssen Sie die Verbindungsinformationen für die Datenbank anpassen.

Die einfachste Möglichkeit zum Einrichten der Datenbank ist die Verwendung der Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp; DisplayLang = En](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ). Mit dem Server verbinden, doppelklicken Sie auf `Databases` und eine neue Datenbank erstellen (mit der rechten Maustaste, und wählen Sie `New Database`) namens `Tutorials`.

In dieser Datenbank, erstellen Sie eine neue Tabelle namens `AJAX` mit den folgenden vier Spalten:

- `id` (primärer Schlüssel, Integer, Identität, nicht NULL)
- `char` (char(1), NULL)
- `description` (varchar(50)-Spalte, NULL)
- `position` (Int, NULL)


[![Das Layout der AJAX-Tabelle](drag-and-drop-via-reorderlist-cs/_static/image2.png)](drag-and-drop-via-reorderlist-cs/_static/image1.png)

Das Layout der AJAX-Tabelle ([klicken Sie, um das Bild in voller Größe anzeigen](drag-and-drop-via-reorderlist-cs/_static/image3.png))


Füllen Sie dann die Tabelle mit ein Paar von Werten. Beachten Sie, dass die `position` -Spalte enthält die Sortierreihenfolge der Elemente.


[![Die ersten Daten in der AJAX-Tabelle](drag-and-drop-via-reorderlist-cs/_static/image5.png)](drag-and-drop-via-reorderlist-cs/_static/image4.png)

Die ersten Daten in der AJAX-Tabelle ([klicken Sie, um das Bild in voller Größe anzeigen](drag-and-drop-via-reorderlist-cs/_static/image6.png))


Der nächste Schritt ist erforderlich, zum Generieren einer `SqlDataSource` Steuerelement für die Kommunikation mit der neuen Datenbank und die Tabelle. Die Datenquelle muss unterstützen die `SELECT` und `UPDATE` SQL-Befehle. Wenn die Reihenfolge der Listenelemente später geändert wird, die `ReorderList` Steuerelement werden automatisch zwei Werte an der Datenquelle sendet `Update` Befehl: die neue Position und die ID des Elements. Aus diesem Grund die Datenquelle muss ein `<UpdateParameters>` Abschnitt für diese beiden Werte:

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample1.aspx)]

Die `ReorderList` Steuerelement muss die folgenden Attribute festzulegen:

- `AllowReorder`: Gibt an, ob Elemente der Liste neu angeordnet werden können
- `DataSourceID`: Die ID der Datenquelle
- `DataKeyField`: Der Name der Primärschlüsselspalte in der Datenquelle
- `SortOrderField`: Die Quellspalte für Daten, die die Sortierreihenfolge für die Listenelemente bereitstellt

In der `<DragHandleTemplate>` und `<ItemTemplate>` Abschnitte, das Layout der Liste angepasst werden kann. Datenbindung ist auch möglich mit der `Eval()` -Methode, wie hier gezeigt:

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample2.aspx)]

Das folgende CSS-Formatinformationen (verwiesen wird, in der `<DragHandleTemplate>` Teil der `ReorderList` Steuerelement) wird sichergestellt, dass sich der Mauszeiger entsprechend ändert, wenn sie über den Ziehpunkt zeigen:

[!code-css[Main](drag-and-drop-via-reorderlist-cs/samples/sample3.css)]

Zum Schluss eine `ScriptManager` Steuerelement initialisiert ASP.NET AJAX für die Seite:

[!code-aspx[Main](drag-and-drop-via-reorderlist-cs/samples/sample4.aspx)]

Führen Sie in diesem Beispiel wird im Browser aus, und ordnen Sie die Listenelemente etwas. Anschließend laden Sie die Seite bzw. verfügen Sie einen Blick auf die Datenbank. Die geänderten Positionen noch gültig und werden auch übernommen, nach den Werten in der `position` -Spalte in die Datenbank ohne Code, nur mithilfe von Markup.


[![Das Datenelement in der Datenbank ändert sich gemäß der neuen Liste Reihenfolge](drag-and-drop-via-reorderlist-cs/_static/image8.png)](drag-and-drop-via-reorderlist-cs/_static/image7.png)

Das Datenelement in der Datenbank ändert sich gemäß der neuen Liste Reihenfolge ([klicken Sie, um das Bild in voller Größe anzeigen](drag-and-drop-via-reorderlist-cs/_static/image9.png))

> [!div class="step-by-step"]
> [Zurück](using-postbacks-with-reorderlist-cs.md)
> [Weiter](using-postbacks-with-reorderlist-vb.md)
