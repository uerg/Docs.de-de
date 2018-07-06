---
uid: web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
title: Verwenden von mehreren Popupsteuerelementen (VB) | Microsoft-Dokumentation
author: wenz
description: Der PopupControl Extender im AJAX Control Toolkit bietet eine einfache Möglichkeit, ein Popup auslösen, wenn ein anderes Steuerelement aktiviert ist. Es ist auch möglich, m verwenden...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: 4da43d77-f6c4-43a8-9124-f1e8e1c8f0a2
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/using-multiple-popup-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 5a2be36ecf17a95d53e5e912ba0a90113f70f2fb
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807634"
---
<a name="using-multiple-popup-controls-vb"></a>Verwenden von mehreren Popupsteuerelementen (VB)
====================
durch [Christian Wenz](https://github.com/wenz)

[Code herunterladen](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl1.vb.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol1VB.pdf)

> Der PopupControl Extender im AJAX Control Toolkit bietet eine einfache Möglichkeit, ein Popup auslösen, wenn ein anderes Steuerelement aktiviert ist. Es ist auch möglich, mehr als eine Popup-Steuerelement auf einer Seite zu verwenden.


## <a name="overview"></a>Übersicht

Der PopupControl Extender im AJAX Control Toolkit bietet eine einfache Möglichkeit, ein Popup auslösen, wenn ein anderes Steuerelement aktiviert ist. Es ist auch möglich, mehr als eine Popup-Steuerelement auf einer Seite zu verwenden.

## <a name="steps"></a>Schritte

Um die Funktionalität von ASP.NET AJAX und das Steuerelement-Toolkit, aktivieren die `ScriptManager` Steuerelement an einer beliebigen Stelle auf der Seite platziert werden muss (jedoch innerhalb der `<form>` Element):

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample1.aspx)]

Fügen Sie einen Bereich, der als das Popup dient. In aktuellen Szenario können der Bereich enthält eine `Calendar` Steuerelement. Um die Seite wird aktualisiert, die aufgrund des Kalenders Postbacks zu vermeiden, platziert der Bereich innerhalb einer `UpdatePanel` Steuerelement:

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample2.aspx)]

Die Seite enthält außerdem zwei Textfelder. Für jedes Textfeld muss Calendar Popupfenster angezeigt, sobald das Textfeld aktiviert ist.

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample3.aspx)]

Jetzt erweitern Sie die beiden Textfelder mit einem `PopupControlExtender`. Die `TargetControlID` Attribut stellt die ID des Steuerelements mit der Extender verknüpft. Die `PopupControlID` Attribut enthält die ID der dem Popupfenster. In diesem Fall sowohl Extender anzeigen im gleichen Bereich, jedoch unterschiedliche Bereiche sind auch möglich.

[!code-aspx[Main](using-multiple-popup-controls-vb/samples/sample4.aspx)]

Nun, wenn Sie in einem Textfeld klicken, wird ein Kalender angezeigt unterhalb des Felds, in dem ein Datum ausgewählt. (Beim Abrufen des ausgewählten Datums zurück in die Textfelder wird in einem anderen Tutorial erläutert.)


[![Der Kalender wird angezeigt, wenn der Benutzer in das Textfeld klickt.](using-multiple-popup-controls-vb/_static/image2.png)](using-multiple-popup-controls-vb/_static/image1.png)

Der Kalender wird angezeigt, wenn der Benutzer in das Textfeld klickt ([klicken Sie, um das Bild in voller Größe anzeigen](using-multiple-popup-controls-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Zurück](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
> [Weiter](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)
