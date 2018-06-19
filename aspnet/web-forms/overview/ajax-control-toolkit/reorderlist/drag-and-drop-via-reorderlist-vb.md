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
ms.locfileid: "30878788"
---
<a name="drag-and-drop-via-reorderlist-vb"></a>Drag & Drop über ReorderList (VB)
====================
durch [Christian Wenz](https://github.com/wenz)

[Herunterladen von Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList5.vb.zip) oder [PDF herunterladen](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist5VB.pdf)

> Das Steuerelement ReorderList im AJAX-Steuerelement-Toolkit bietet eine Liste, die vom Benutzer per Drag & Drop neu angeordnet werden kann. Die aktuelle Bestellung in der Liste muss auf dem Server beibehalten werden.


## <a name="overview"></a>Übersicht

Die `ReorderList` Steuerelement im AJAX-Steuerelement-Toolkit enthält eine Liste, die vom Benutzer per Drag & Drop neu angeordnet werden kann. Die aktuelle Bestellung in der Liste muss auf dem Server beibehalten werden.

## <a name="steps"></a>Schritte

Die `ReorderList` Steuerelement unterstützt Binden von Daten aus einer Datenbank in der Liste. Beste daran ist, wird auch das Schreiben von Änderungen Sequenznummern das Listenelement zurück an den Datenspeicher unterstützt.

Dieses Beispiel verwendet Microsoft SQL Server 2005 Express Edition als Datenspeicher. Die Datenbank ist ein optional (und kostenlosen) Teil einer Installation von Visual Studio, einschließlich der express Edition. Es ist auch verfügbar als separater Download unter [ https://go.microsoft.com/fwlink/?LinkId=64064 ](https://go.microsoft.com/fwlink/?LinkId=64064). In diesem Beispiel gehen wir davon aus, dass die Instanz von SQL Server 2005 Express Edition aufgerufen wird `SQLEXPRESS` und befindet sich auf dem gleichen Computer wie der Webserver; Dies ist auch die Standardeinstellungen. Wenn Ihr Setup abweicht, müssen Sie passen Sie die Verbindungsinformationen für die Datenbank.

Die einfachste Möglichkeit zum Einrichten der Datenbank ist die Verwendung der Microsoft SQL Server Management Studio Express ([https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang = En](https://www.microsoft.com/downloads/details.aspx?FamilyID=c243a5ae-4bd1-4e3d-94b8-5a0f62bf7796&amp;DisplayLang=en) ). Herstellen einer Verbindung mit dem Server, doppelklicken Sie auf `Databases` und eine neue Datenbank erstellen (mit der rechten Maustaste, und wählen Sie `New Database`) aufgerufen `Tutorials`.

Erstellen Sie in dieser Datenbank eine neue Tabelle namens `AJAX` mit den folgenden vier Spalten:

- `id` (primary Key, Integer, Identität, not NULL)
- `char` (char(1), NULL)
- `description` (varchar(50)-Spalte, NULL)
- `position` (int, NULL)


[![Das Layout der AJAX-Tabelle](drag-and-drop-via-reorderlist-vb/_static/image2.png)](drag-and-drop-via-reorderlist-vb/_static/image1.png)

Das Layout der AJAX-Tabelle ([klicken Sie hier, um das Bild in voller Größe angezeigt](drag-and-drop-via-reorderlist-vb/_static/image3.png))


Als Nächstes werden füllen Sie die Tabelle über ein Paar von Werten. Beachten Sie, dass die `position` Spalte enthält die Sortierreihenfolge der Elemente.


[![Die ursprünglichen Daten in der AJAX-Tabelle](drag-and-drop-via-reorderlist-vb/_static/image5.png)](drag-and-drop-via-reorderlist-vb/_static/image4.png)

Die ursprünglichen Daten in der AJAX-Tabelle ([klicken Sie hier, um das Bild in voller Größe angezeigt](drag-and-drop-via-reorderlist-vb/_static/image6.png))


Der nächste Schritt ist erforderlich, zum Generieren einer `SqlDataSource` Steuerelement für die Kommunikation mit der neuen Datenbank und die Tabelle. Die Datenquelle muss unterstützen die `SELECT` und `UPDATE` SQL-Befehle. Wenn die Reihenfolge der Listenelemente später geändert wird, die `ReorderList` Steuerelement sendet automatisch zwei Werte an der Datenquelle `Update` Befehl: die neue Position und die ID des Elements. Aus diesem Grund die Datenquelle muss ein `<UpdateParameters>` Abschnitt, um diese beiden Werte:

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample1.aspx)]

Die `ReorderList` Steuerelement muss die folgenden Attribute festzulegen:

- `AllowReorder`: Gibt an, ob die Elemente der Liste neu angeordnet werden können
- `DataSourceID`: Die ID der Datenquelle
- `DataKeyField`: Der Name der Primärschlüsselspalte in der Datenquelle
- `SortOrderField`: Die Datenquellenspalte, die die Sortierreihenfolge für die Listenelemente bereitstellt.

In der `<DragHandleTemplate>` und `<ItemTemplate>` Abschnitte, das Layout der Liste kann optimiert werden. Datenbindung ist außerdem möglich mit der `Eval()` -Methode, wie hier zu sehen:

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample2.aspx)]

Das folgende CSS-Formatinformationen (verwiesen wird, in der `<DragHandleTemplate>` Teil der `ReorderList` Steuerelement) wird sichergestellt, dass der Mauszeiger die Form entsprechend geändert wird, wenn er über dem Ziehpunkt bewegt wird:

[!code-css[Main](drag-and-drop-via-reorderlist-vb/samples/sample3.css)]

Schließlich ein `ScriptManager` Steuerelement initialisiert ASP.NET AJAX für die Seite:

[!code-aspx[Main](drag-and-drop-via-reorderlist-vb/samples/sample4.aspx)]

Führen Sie in diesem Beispiel wird im Browser, und ordnen Sie die Listenelemente etwas. Klicken Sie dann die Seite neu zu laden, und/oder einen Blick auf die Datenbank verfügen. Die geänderten Positionen gewartet wurden und auch nach den Werten in wiedergegeben werden die `position` Spalte in der Datenbank und, ohne Code, nur mithilfe von Markup.


[![Die Daten in der Datenbank ändert sich entsprechend der Reihenfolge der neuen Liste-Element](drag-and-drop-via-reorderlist-vb/_static/image8.png)](drag-and-drop-via-reorderlist-vb/_static/image7.png)

Das Datenelement in der Datenbank geändert wird, gemäß der neuen Liste Reihenfolge ([klicken Sie hier, um das Bild in voller Größe angezeigt](drag-and-drop-via-reorderlist-vb/_static/image9.png))

> [!div class="step-by-step"]
> [Vorherige](using-postbacks-with-reorderlist-vb.md)
