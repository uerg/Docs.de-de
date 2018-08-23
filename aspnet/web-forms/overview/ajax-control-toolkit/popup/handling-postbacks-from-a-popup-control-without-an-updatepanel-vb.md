---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
title: Verarbeiten von Postbacks über ein Popupsteuerelement ohne ein UpdatePanel-Steuerelement (VB) | Microsoft-Dokumentation
author: wenz
description: Der PopupControl Extender im AJAX Control Toolkit bietet eine einfache Möglichkeit, ein Popup auslösen, wenn ein anderes Steuerelement aktiviert ist. Wenn ein Postback in "su" erfolgt...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: a0b9186c-0912-4fff-916a-6d17e696a50b
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: 6dcb2b09279ec6400465f79fadc2a1b6c72f8f07
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836466"
---
<a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-vb"></a>Verarbeiten von Postbacks über ein Popupsteuerelement ohne ein UpdatePanel-Steuerelement (VB)
====================
durch [Christian Wenz](https://github.com/wenz)

[Code herunterladen](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.vb.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3VB.pdf)

> Der PopupControl Extender im AJAX Control Toolkit bietet eine einfache Möglichkeit, ein Popup auslösen, wenn ein anderes Steuerelement aktiviert ist. Wenn ein Postback, in einem Bereich angezeigt auftritt, und es mehrere Bereiche auf der Seite gibt ist es schwierig, um zu bestimmen, welchen Bereich geklickt wurde.


## <a name="overview"></a>Übersicht

Der PopupControl Extender im AJAX Control Toolkit bietet eine einfache Möglichkeit, ein Popup auslösen, wenn ein anderes Steuerelement aktiviert ist. Wenn ein Postback, in einem Bereich angezeigt auftritt, und es mehrere Bereiche auf der Seite gibt ist es schwierig, um zu bestimmen, welchen Bereich geklickt wurde.

## <a name="steps"></a>Schritte

Bei Verwendung einer `PopupControl` mit einem Postback, aber ohne eine `UpdatePanel` auf der Seite das Steuerelement-Toolkit nicht bietet eine Möglichkeit zu bestimmen, welche Client-Element des Popups ausgelöst hat, der wiederum das Postback verursacht hat. Jedoch ein kleinen Trick zur Umgehung dieses Problems für dieses Szenario bietet.

Zunächst sieht die grundlegende Einrichtung: zwei Textfelder, die beide die gleiche Popup, einen Kalender auslösen. Zwei `PopupControlExtenders` Vereinen von Textfeldern und den popupausrichtungspunkt.

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample1.aspx)]

Die grundlegende Idee ist, Hinzufügen einer ausgeblendeten Formularfelds in der &lt; `form` &gt; -Element, das das Textfeld enthält, die das Popup gestartet:

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample2.aspx)]

Wenn die Seite geladen wird, fügt JavaScript-Code einen Ereignishandler auf die beiden Textfelder: Sobald der Benutzer auf das Textfeld klickt, wird der Name in ausgeblendeten Formularfelds geschrieben:

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample3.html)]

In den serverseitigen Code verwenden muss der Wert des ausgeblendeten Felds gelesen werden. Da ausgeblendeten Formularfeldern sehr einfach zu bearbeiten sind, ist ein Whitelist-Ansatz zur Überprüfung des Werts der ausgeblendeten erforderlich. Nachdem das Textfeld für die richtige identifiziert wurde, wird das Datum aus dem Kalender in diese geschrieben.

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample4.aspx)]


[![Der Kalender wird angezeigt, wenn der Benutzer in das Textfeld klickt.](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image1.png)

Der Kalender wird angezeigt, wenn der Benutzer in das Textfeld klickt ([klicken Sie, um das Bild in voller Größe anzeigen](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image3.png))


[![Durch Klicken auf ein Datum in das Textfeld eingefügt](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image4.png)

Durch Klicken auf ein Datum in das Textfeld eingefügt ([klicken Sie, um das Bild in voller Größe anzeigen](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Vorherige](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)
