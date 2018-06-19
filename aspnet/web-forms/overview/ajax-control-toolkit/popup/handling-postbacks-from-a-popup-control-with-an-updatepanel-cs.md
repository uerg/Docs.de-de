---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
title: Behandlung von Postbacks aus einem Popupsteuerelement mit UpdatePanel (c#) | Microsoft Docs
author: wenz
description: Der Extender PopupControl im AJAX Control Toolkit bietet eine einfache Möglichkeit, um ein Popup auszulösen, wenn es sich bei jedem anderen Steuerelement aktiviert wird. Besondere Sorgfalt hat auszuführende...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 1f68f59d-9c1e-4cf3-b304-c13ae6b7203e
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-with-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: abedb5247f710b02752651a7bfb011ab63d32844
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879633"
---
<a name="handling-postbacks-from-a-popup-control-with-an-updatepanel-c"></a>Behandlung von Postbacks aus einem Popupsteuerelement mit UpdatePanel (c#)
====================
durch [Christian Wenz](https://github.com/wenz)

[Herunterladen von Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl2.cs.zip) oder [PDF herunterladen](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol2CS.pdf)

> Der Extender PopupControl im AJAX Control Toolkit bietet eine einfache Möglichkeit, um ein Popup auszulösen, wenn es sich bei jedem anderen Steuerelement aktiviert wird. Besondere Sorgfalt muss ausgeführt werden, wenn ein Postback solche ein Popup erfolgt.


## <a name="overview"></a>Übersicht

Der Extender PopupControl im AJAX Control Toolkit bietet eine einfache Möglichkeit, um ein Popup auszulösen, wenn es sich bei jedem anderen Steuerelement aktiviert wird. Besondere Sorgfalt muss ausgeführt werden, wenn ein Postback solche ein Popup erfolgt.

## <a name="steps"></a>Schritte

Bei Verwendung einer `PopupControl` mit einem Postback ein `UpdatePanel` können verhindern, dass die Seite-Aktualisierung, die durch das Postback verursacht. Das folgende Markup definiert eine Reihe von wichtigen Elemente:

- Ein `ScriptManager` steuern, sodass das ASP.NET AJAX-Steuerelement-Toolkit funktioniert
- Zwei `TextBox` steuert, welche sowohl ein Popup ausgelöst werden
- Ein `Panel` Steuerelement, das als das Popup fungieren soll
- Innerhalb des Bereichs einer `Calendar` Steuerelement eingebettet ist ein `UpdatePanel` Steuerelement
- Zwei `PopupControlExtender` Steuerelemente, die im Bereich der Textfelder zuweisen

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample1.aspx)]

Beachten Sie, dass die `OnSelectionChanged` Attribut von der `Calendar` Steuerelement festgelegt ist. Damit ein Postback tritt auf, wenn der Benutzer ein Datum im Kalender auswählt, und die serverseitige Methode `c1_SelectionChanged()` ausgeführt wird. Innerhalb dieser Methode muss das aktuelle Datum abgerufen und zurück in das Textfeld geschrieben werden.

Die Syntax lautet wie folgt: Erstens einen Proxy-Objekt für die `PopupControlExtender` auf der Seite generiert werden muss. Das ASP.NET AJAX-Steuerelement-Toolkit bietet die `GetProxyForCurrentPopup()` Methode. Das Objekt, diese Methode gibt, unterstützt die `Commit()` Methode einen Wert an das Steuerelement gesendet wird, die das Popup (nicht das Steuerelement, das dem Aufruf der Methode ausgelöst!) ausgelöst. Der folgende Code stellt das ausgewählte Datum als Argument für die `Commit()` -Methode, sodass des Codes zum Zurückschreiben von des ausgewählten Datums in das Textfeld:

[!code-aspx[Main](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/samples/sample2.aspx)]

Jetzt immer an einem Kalenderdatum klicken Sie auf das ausgewählte Datum angezeigt wird, klicken Sie im zugehörigen Textfeld kann ein Datumsauswahl-Steuerelement erstellen, die derzeit auf vielen Websites gefunden werden.


[![Der Kalender wird angezeigt, wenn der Benutzer in das Textfeld klickt](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image1.png)

Der Kalender wird angezeigt, wenn der Benutzer in das Textfeld klickt ([klicken Sie hier, um das Bild in voller Größe angezeigt](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image3.png))


[![Durch Klicken auf ein Datum in das Textfeld eingefügt](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image4.png)

Durch Klicken auf ein Datum in das Textfeld eingefügt ([klicken Sie hier, um das Bild in voller Größe angezeigt](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs/_static/image6.png))

> [!div class="step-by-step"]
> [Zurück](using-multiple-popup-controls-cs.md)
> [Weiter](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs.md)
