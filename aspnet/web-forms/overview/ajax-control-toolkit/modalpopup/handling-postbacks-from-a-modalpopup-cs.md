---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
title: Verarbeiten von Postbacks über ein ModalPopup-Steuerelement (c#) | Microsoft-Dokumentation
author: wenz
description: Der ModalPopup-Steuerelement im AJAX Control Toolkit bietet eine einfache Möglichkeit, ein modales Fenster mithilfe der clientseitigen Methoden zu erstellen. Besondere Sorgfalt muss ausgeführt werden, wenn ein pos...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 7963890b-4ea3-4a1c-b65d-6098a3d56f62
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/handling-postbacks-from-a-modalpopup-cs
msc.type: authoredcontent
ms.openlocfilehash: 9bcb1ad058b800f3f957cafff07a5a54b44178a6
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41823948"
---
<a name="handling-postbacks-from-a-modalpopup-c"></a>Verarbeiten von Postbacks über ein ModalPopup-Steuerelement (c#)
====================
durch [Christian Wenz](https://github.com/wenz)

[Code herunterladen](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup3.cs.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup3CS.pdf)

> Der ModalPopup-Steuerelement im AJAX Control Toolkit bietet eine einfache Möglichkeit, ein modales Fenster mithilfe der clientseitigen Methoden zu erstellen. Besondere Sorgfalt muss ausgeführt werden, wenn ein Postback aus in das Popup erstellt wird.


## <a name="overview"></a>Übersicht

Der ModalPopup-Steuerelement im AJAX Control Toolkit bietet eine einfache Möglichkeit, ein modales Fenster mithilfe der clientseitigen Methoden zu erstellen. Besondere Sorgfalt muss ausgeführt werden, wenn ein Postback aus in das Popup erstellt wird.

## <a name="steps"></a>Schritte

Um die Funktionalität von ASP.NET AJAX und das Steuerelement-Toolkit, aktivieren die `ScriptManager` Steuerelement an einer beliebigen Stelle auf der Seite platziert werden muss (jedoch innerhalb der `<form>` Element):

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample1.aspx)]

Fügen Sie einen Bereich, der als modales Fenster dient. Dort kann der Benutzer einen Namen und eine e-Mail-Adresse eingeben. Eine Schaltfläche wird verwendet, um das Popup zu schließen und speichern Sie die Informationen. Beachten Sie, dass die `OnClick` -Attribut festgelegt ist, sodass ein Postback auftritt, wenn auf diese Schaltfläche geklickt wird:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample2.aspx)]

Die Seite selbst besteht aus zwei Bezeichnungen für genau die gleichen Informationen: Name und e-Mail-Adresse. Eine Schaltfläche dient zum modalen Popups auslösen:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample3.aspx)]

Um das Popup angezeigt werden können, fügen die `ModalPopupExtender` Steuerelement. Legen Sie die `PopupControlID` Attribut des Bereichs-ID und `TargetControlID` auf die Schaltfläche "ID:

[!code-aspx[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample4.aspx)]

Jetzt immer die `Save` innerhalb des modalen Popups geklickt wird, die serverseitige `SaveData()` Methode ausgeführt wird. Dort können Sie die eingegebenen Daten in einem Datenspeicher sparen. Der Einfachheit halber werden die neuen Daten nur in der Bezeichnung Ausgabe:

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample5.cs)]

Darüber hinaus sollte die Textbox-Steuerelemente innerhalb des modalen Popups mit den aktuellen Namen und e-Mail-Adresse gefüllt werden. Dies ist jedoch nur erforderlich, wenn kein Postback auftritt. Ist es ein Postback, füllt die Viewstate-Funktion von ASP.NET automatisch die Textfelder ein, durch die entsprechenden Werte.

[!code-csharp[Main](handling-postbacks-from-a-modalpopup-cs/samples/sample6.cs)]


[![Modale Fenster auslöst ein Postback.](handling-postbacks-from-a-modalpopup-cs/_static/image2.png)](handling-postbacks-from-a-modalpopup-cs/_static/image1.png)

Modale Fenster ein Postback auslöst ([klicken Sie, um das Bild in voller Größe anzeigen](handling-postbacks-from-a-modalpopup-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Zurück](using-modalpopup-with-a-repeater-control-cs.md)
> [Weiter](positioning-a-modalpopup-cs.md)
