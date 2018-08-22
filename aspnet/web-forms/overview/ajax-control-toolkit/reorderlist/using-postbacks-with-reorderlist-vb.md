---
uid: web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
title: Verwenden von Postbacks mit dem ReorderList-Steuerelement (VB) | Microsoft-Dokumentation
author: wenz
description: Die ReorderList-Steuerelement im AJAX Control Toolkit enthält eine Liste, die durch den Benutzer über Drag & Drop neu angeordnet werden kann. Wenn die Liste neu angeordnet wird, eine Bestellung...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: e5b6ed70-19ed-4024-ba4f-6d78e8acdc0f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/reorderlist/using-postbacks-with-reorderlist-vb
msc.type: authoredcontent
ms.openlocfilehash: 1e2124327a36c94db8bafbdf91f767068ac7e834
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41826799"
---
<a name="using-postbacks-with-reorderlist-vb"></a>Verwenden von Postbacks mit dem ReorderList-Steuerelement (VB)
====================
durch [Christian Wenz](https://github.com/wenz)

[Code herunterladen](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.vb.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4VB.pdf)

> Die ReorderList-Steuerelement im AJAX Control Toolkit enthält eine Liste, die durch den Benutzer über Drag & Drop neu angeordnet werden kann. Wenn die Liste neu angeordnet wird, unterrichtet ein Postback den Server über die Änderung.


## <a name="overview"></a>Übersicht

Die `ReorderList` Steuerelement im AJAX Control Toolkit enthält eine Liste, die durch den Benutzer über Drag & Drop neu angeordnet werden kann. Wenn die Liste neu angeordnet wird, unterrichtet ein Postback den Server über die Änderung.

## <a name="steps"></a>Schritte

Es gibt mehrere mögliche Datenquellen für die `ReorderList` Steuerelement. Eine ist die Verwendung einer `XmlDataSource` Steuerelement:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample1.aspx)]

Zum Binden dieser XML-Code, um eine `ReorderList` Postbacks an Kontrolle und aktivieren, die folgenden Attribute müssen festgelegt werden:

- `DataSourceID`: Die ID der Datenquelle
- `SortOrderField`: Die Eigenschaft, um nach zu sortieren
- `AllowReorder`: Angibt, ob der Benutzer auf die Listenelemente neu anordnen.
- `PostBackOnReorder`: Angibt, ob einen Postback zu erstellen, wenn die Liste neu angeordnet werden

Hier ist das entsprechende Markup für das Steuerelement ein:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample2.aspx)]

In der `ReorderList` -Steuerelement, bestimmte Daten aus der Datenquelle kann gebunden werden, mithilfe der `Eval()` Methode:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample3.aspx)]

An einer beliebigen Stelle auf der Seite wird eine Bezeichnung auf die Informationen aufnimmt, wenn die letzte Neuordnung aufgetreten ist:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample4.aspx)]

Diese Bezeichnung wird mit Text in den serverseitigen Code, der Verarbeitung des Postbacks gefüllt:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample5.aspx)]

Zum Schluss, um die Funktionalität von ASP.NET AJAX und das Steuerelement-Toolkit, aktivieren die `ScriptManager` Steuerelement auf der Seite platziert werden muss:

[!code-aspx[Main](using-postbacks-with-reorderlist-vb/samples/sample6.aspx)]


[![Jede Neuordnung auslöst einen postback](using-postbacks-with-reorderlist-vb/_static/image2.png)](using-postbacks-with-reorderlist-vb/_static/image1.png)

Jede Neuordnung einen Postback auslöst ([klicken Sie, um das Bild in voller Größe anzeigen](using-postbacks-with-reorderlist-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Zurück](drag-and-drop-via-reorderlist-cs.md)
> [Weiter](drag-and-drop-via-reorderlist-vb.md)
