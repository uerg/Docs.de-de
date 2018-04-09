---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
title: Verwenden mehrere Popup-Steuerelementen (VB) | Microsoft Docs
author: wenz
description: Der Extender PopupControl im AJAX Control Toolkit bietet eine einfache Möglichkeit, um ein Popup auszulösen, wenn es sich bei jedem anderen Steuerelement aktiviert wird. Es ist auch möglich, verwenden Sie m...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 4da43d77-f6c4-43a8-9124-f1e8e1c8f0a2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 7c57aab3ecf2c02a8488b5ea4e3e0ed33ac5e7fe
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="using-multiple-popup-controls-vb"></a>Verwenden von mehreren Popup-Steuerelementen (VB)
====================
durch [Christian Wenz](https://github.com/wenz)

[Herunterladen von Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.vb.zip) oder [PDF herunterladen](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1VB.pdf)

> Der Extender PopupControl im AJAX Control Toolkit bietet eine einfache Möglichkeit, um ein Popup auszulösen, wenn es sich bei jedem anderen Steuerelement aktiviert wird. Es ist auch möglich, mehr als ein Popupsteuerelement auf einer Seite zu verwenden.


## <a name="overview"></a>Übersicht

Der Extender PopupControl im AJAX Control Toolkit bietet eine einfache Möglichkeit, um ein Popup auszulösen, wenn es sich bei jedem anderen Steuerelement aktiviert wird. Es ist auch möglich, mehr als ein Popupsteuerelement auf einer Seite zu verwenden.

## <a name="steps"></a>Schritte

Um die Funktionalität von ASP.NET AJAX und das Steuerelement-Toolkit aktivieren die `ScriptManager` Steuerelement an einer beliebigen Stelle auf der Seite versetzt werden muss (jedoch innerhalb der `<form>` Element):

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample1.aspx)]

Fügen Sie ein Panel die als das Popup dient. In das aktuelle Szenario Bereich enthält eine `Calendar` Steuerelement. Um die Seite wird aktualisiert, die durch den Kalender Postbacks verursacht zu vermeiden, wird im Bereich eingefügt, innerhalb einer `UpdatePanel` Steuerelement:

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample2.aspx)]

Die Seite enthält außerdem zwei Textfelder. Für jedes Textfeld muss das Kalender-Popup angezeigt, wenn das Textfeld aktiviert ist.

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample3.aspx)]

Jetzt erweitern Sie die beiden Textfelder mit einem `PopupControlExtender`. Die `TargetControlID` Attribut stellt die ID des Steuerelements an den Extender gebunden sind. Die `PopupControlID` Attribut enthält die ID der Popup-Fenster. In diesem Fall beide Extender anzeigen im gleichen Bereich, aber unterschiedliche Bereichen sind ebenfalls möglich.

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample4.aspx)]

Jetzt sobald Sie in einem Textfeld klicken, erscheint ein Kalender unterhalb des Felds ein Datum auswählen. (Beim Abrufen des ausgewählten Datums zurück in die Textfelder werden in einem anderen Lernprogramm behandelt.)


[![Der Kalender wird angezeigt, wenn der Benutzer in das Textfeld klickt](using-multiple-popup-controls-vb/_static/image2.png)](using-multiple-popup-controls-vb/_static/image1.png)

Der Kalender wird angezeigt, wenn der Benutzer in das Textfeld klickt ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-multiple-popup-controls-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Zurück](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
> [Weiter](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)
