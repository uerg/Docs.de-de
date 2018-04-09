---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
title: Behandlung von Postbacks aus einem ModalPopup (c#) | Microsoft Docs
author: wenz
description: ModalPopup-Steuerelement in das AJAX-Steuerelement-Toolkit bietet eine einfache Möglichkeit, ein modales Popupdialogfeld clientseitige Weise zu erstellen. Muss bei einem pos darauf geachtet werden...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 7963890b-4ea3-4a1c-b65d-6098a3d56f62
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: 183725db62ba8b4037f368ed9d87d5059e3f1bcb
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="handling-postbacks-from-a-modalpopup-c"></a>Behandlung von Postbacks aus einem ModalPopup (c#)
====================
durch [Christian Wenz](https://github.com/wenz)

[Herunterladen von Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.cs.zip) oder [PDF herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3CS.pdf)

> ModalPopup-Steuerelement in das AJAX-Steuerelement-Toolkit bietet eine einfache Möglichkeit, ein modales Popupdialogfeld clientseitige Weise zu erstellen. Muss darauf geachtet werden, wenn ein Postback aus innerhalb der Meldung erstellt wird.


## <a name="overview"></a>Übersicht

ModalPopup-Steuerelement in das AJAX-Steuerelement-Toolkit bietet eine einfache Möglichkeit, ein modales Popupdialogfeld clientseitige Weise zu erstellen. Muss darauf geachtet werden, wenn ein Postback aus innerhalb der Meldung erstellt wird.

## <a name="steps"></a>Schritte

Um die Funktionalität von ASP.NET AJAX und das Steuerelement-Toolkit aktivieren die `ScriptManager` Steuerelement an einer beliebigen Stelle auf der Seite versetzt werden muss (jedoch innerhalb der `<form>` Element):

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample1.aspx)]

Fügen Sie ein Panel die als modales Popupdialogfeld dient. Vorhanden ist, kann der Benutzer einen Namen und eine e-Mail-Adresse eingeben. Eine Schaltfläche wird verwendet, um das Popup zu schließen und die Informationen speichern. Beachten Sie, dass die `OnClick` -Attribut festgelegt ist, sodass ein Postback tritt auf, wenn auf diese Schaltfläche geklickt wird:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample2.aspx)]

Die Seite selbst besteht aus zwei Bezeichnungen für genau die gleichen Informationen an: Name und e-Mail-Adresse. Eine Schaltfläche wird verwendet, um die modales Popupdialogfeld auslösen:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample3.aspx)]

Um das Popupfenster angezeigt zu machen, fügen die `ModalPopupExtender` Steuerelement. Legen Sie die `PopupControlID` -Attribut des Bereichs-ID und `TargetControlID` auf die Schaltfläche-ID:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample4.aspx)]

Jetzt immer die `Save` innerhalb der modales Popupdialogfeld geklickt wird, die serverseitige `SaveData()` Methode ausgeführt wird. Vorhanden ist, konnten Sie die eingegebenen Daten in einem Datenspeicher gespeichert. Der Einfachheit halber nur die neuen Daten in der Bezeichnung ausgegeben:

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample5.cs)]

Darüber hinaus sollte das Textbox-Steuerelemente innerhalb der modales Popupdialogfeld mit dem aktuellen Namen und e-Mail-gefüllt werden. Dies ist jedoch nur erforderlich, wenn kein Postback auftritt. Ist ein Postback, werden die ASP.NET-Funktion "ViewState" speichern die Textfelder ein, durch die entsprechenden Werte automatisch ausgefüllt.

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample6.cs)]


[![Modale Popupdialogfeld verursacht einen Postback.](handling-postbacks-from-a-modalpopup-cs/_static/image2.png)](handling-postbacks-from-a-modalpopup-cs/_static/image1.png)

Modale Popupdialogfeld bewirkt, dass einen Postback ([klicken Sie hier, um das Bild in voller Größe angezeigt](handling-postbacks-from-a-modalpopup-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Zurück](using-modalpopup-with-a-repeater-control-cs.md)
> [Weiter](positioning-a-modalpopup-cs.md)
