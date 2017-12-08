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
<a name="using-postbacks-with-reorderlist-c"></a>Verwenden von Postbacks mit ReorderList (c#)
====================
durch [Christian Wenz](https://github.com/wenz)

[Herunterladen von Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/ReorderList4.cs.zip) oder [PDF herunterladen](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/reorderlist4CS.pdf)

> Das Steuerelement ReorderList im AJAX-Steuerelement-Toolkit bietet eine Liste, die vom Benutzer per Drag & Drop neu angeordnet werden kann. Wenn die Liste neu sortiert wird, teilt ein Postback den Server über die Änderung.


## <a name="overview"></a>Übersicht

Die `ReorderList` Steuerelement im AJAX-Steuerelement-Toolkit enthält eine Liste, die vom Benutzer per Drag & Drop neu angeordnet werden kann. Wenn die Liste neu sortiert wird, teilt ein Postback den Server über die Änderung.

## <a name="steps"></a>Schritte

Es gibt mehrere mögliche Datenquellen für die `ReorderList` Steuerelement. Verwenden einer `XmlDataSource` Steuerelement:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample1.aspx)]

Um diesen XML-Code zum Binden einer `ReorderList` Postbacks-Steuerelement, und aktivieren, die folgenden Attribute müssen festgelegt werden:

- `DataSourceID`: Die ID der Datenquelle
- `SortOrderField`: Die Eigenschaft Sortierungskriterium
- `AllowReorder`: Angibt, ob der Benutzer auf die Listenelemente neu anordnen.
- `PostBackOnReorder`: Angibt, ob einen Postback erstellen, wenn die Liste neu angeordnet ist

So sieht das entsprechende Markup für das Steuerelement aus:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample2.aspx)]

Innerhalb der `ReorderList` Steuerelement bestimmte Daten aus der Datenquelle kann gebunden werden, mithilfe der `Eval()` Methode:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample3.aspx)]

An einer beliebigen Stelle auf der Seite wird eine Bezeichnung die Informationen enthalten, bei der letzten Neuanordnen von aufgetreten ist:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample4.aspx)]

Diese Bezeichnung wird Text im serverseitigen Code behandeln das Postback eingetragen:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample5.aspx)]

Schließlich, um die Funktionalität von ASP.NET AJAX und das Steuerelement-Toolkit aktivieren die `ScriptManager` Steuerelement muss auf der Seite platziert werden:

[!code-aspx[Main](using-postbacks-with-reorderlist-cs/samples/sample6.aspx)]


[![Jede Neuanordnen von löst einen postback](using-postbacks-with-reorderlist-cs/_static/image2.png)](using-postbacks-with-reorderlist-cs/_static/image1.png)

Jede Neuanordnen von löst einen Postback ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-postbacks-with-reorderlist-cs/_static/image3.png))

>[!div class="step-by-step"]
[Nächste](drag-and-drop-via-reorderlist-cs.md)
