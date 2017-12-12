---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-cs
title: Positionieren ein ModalPopup (c#) | Microsoft Docs
author: wenz
description: "ModalPopup-Steuerelement in das AJAX-Steuerelement-Toolkit bietet eine einfache Möglichkeit, ein modales Popupdialogfeld clientseitige Weise zu erstellen. Das Steuerelement jedoch keine bietet ein..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 1caac9d0-e21e-49d6-a8ff-e563a736d6ca
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/positioning-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: 8dcc4e20ac98cbbad1ea3e86b7f895d32c853d4a
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="positioning-a-modalpopup-c"></a>Positionieren ein ModalPopup (c#)
====================
durch [Christian Wenz](https://github.com/wenz)

[Herunterladen von Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup4.cs.zip) oder [PDF herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup4CS.pdf)

> ModalPopup-Steuerelement in das AJAX-Steuerelement-Toolkit bietet eine einfache Möglichkeit, ein modales Popupdialogfeld clientseitige Weise zu erstellen. Das Steuerelement bietet jedoch nicht über eine integrierte Funktionalität, um das Popup zu positionieren.


## <a name="overview"></a>Übersicht

ModalPopup-Steuerelement in das AJAX-Steuerelement-Toolkit bietet eine einfache Möglichkeit, ein modales Popupdialogfeld clientseitige Weise zu erstellen. Das Steuerelement bietet jedoch nicht über eine integrierte Funktionalität, um das Popup zu positionieren.

## <a name="steps"></a>Schritte

Um die Funktionalität von ASP.NET AJAX und das Steuerelement-Toolkit aktivieren die `ScriptManager`. Steuerelement an einer beliebigen Stelle auf der Seite versetzt werden muss (jedoch innerhalb der `<form>` Element):

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample1.aspx)]

Fügen Sie ein Panel die als modales Popupdialogfeld dient. Eine Schaltfläche wird verwendet, um das Popup zu schließen:

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample2.aspx)]

Wenn das Popupfenster angezeigt wird, muss er an einem bestimmten Ort auf der Seite positioniert werden. Für diese Aufgabe wird eine clientseitige JavaScript-Funktion erstellt. Zuerst wird versucht, den Bereich zu gelangen. Wenn er erfolgreich ausgeführt wird, wird im Bereich positioniert mit CSS und JavaScript (ändern sich die Position des Popups an). Jedoch `ModalPopupExtender` Steuerelement auch versucht wird, um das Popup zu positionieren. Aus diesem Grund positioniert der JavaScript-Code wiederholt das Popupfenster alle Zehntelsekunde.

[!code-html[Main](positioning-a-modalpopup-cs/samples/sample3.html)]

Wie können Sie sehen, den Rückgabewert der `setTimeout()` JavaScript-Methode in einer globalen Variablen gespeichert ist. Auf diese Weise können die wiederholte Positionierung des Popups bei Bedarf beenden mithilfe der `clearTimeout()` Methode:

[!code-javascript[Main](positioning-a-modalpopup-cs/samples/sample4.js)]

Jetzt noch hierzu einen besteht darin, den Browser, die diese Funktionen jederzeit aufrufen. Die `movePanel()` JavaScript-Funktion muss aufgerufen werden, wenn die Schaltfläche geklickt wird, die im Bereich auslöst:

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample5.aspx)]

Und die `stopMoving()` Funktion kommt es zu einem geschlossenem das Popup Dies kann ausgelöst werden, der `ModalPopupExtender` Steuerelement:

[!code-aspx[Main](positioning-a-modalpopup-cs/samples/sample6.aspx)]


[![Das modale Popupfenster angezeigt wird, an der angegebenen position](positioning-a-modalpopup-cs/_static/image2.png)](positioning-a-modalpopup-cs/_static/image1.png)

Das modale Popupfenster angezeigt wird, an der angegebenen Position ([klicken Sie hier, um das Bild in voller Größe angezeigt](positioning-a-modalpopup-cs/_static/image3.png))

>[!div class="step-by-step"]
[Zurück](handling-postbacks-from-a-modalpopup-cs.md)
[Weiter](launching-a-modal-popup-window-from-server-code-vb.md)
