---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
title: Behandlung von Postbacks aus einem Popupsteuerelement ohne UpdatePanel (VB) | Microsoft Docs
author: wenz
description: Der Extender PopupControl im AJAX Control Toolkit bietet eine einfache Möglichkeit, um ein Popup auszulösen, wenn es sich bei jedem anderen Steuerelement aktiviert wird. Tritt ein Postback in "su"...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: a0b9186c-0912-4fff-916a-6d17e696a50b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-vb
msc.type: authoredcontent
ms.openlocfilehash: c92083d4a57067c7b646721f21af059fc1e1e7d9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30871362"
---
<a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-vb"></a>Behandlung von Postbacks aus einem Popupsteuerelement ohne UpdatePanel (VB)
====================
durch [Christian Wenz](https://github.com/wenz)

[Herunterladen von Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.vb.zip) oder [PDF herunterladen](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3VB.pdf)

> Der Extender PopupControl im AJAX Control Toolkit bietet eine einfache Möglichkeit, um ein Popup auszulösen, wenn es sich bei jedem anderen Steuerelement aktiviert wird. Wenn ein Postback in einem solchen Bereich erfolgt und ggf. mehrere Bereiche auf der Seite werden ist es schwierig, zu bestimmen, welcher Bereich geklickt wurde.


## <a name="overview"></a>Übersicht

Der Extender PopupControl im AJAX Control Toolkit bietet eine einfache Möglichkeit, um ein Popup auszulösen, wenn es sich bei jedem anderen Steuerelement aktiviert wird. Wenn ein Postback in einem solchen Bereich erfolgt und ggf. mehrere Bereiche auf der Seite werden ist es schwierig, zu bestimmen, welcher Bereich geklickt wurde.

## <a name="steps"></a>Schritte

Bei Verwendung einer `PopupControl` mit einem Postback, aber ohne eine `UpdatePanel` auf der Seite die Control-Toolkit nicht bieten eine Möglichkeit zu bestimmen, welches Clientelement das Popup ausgelöst wurde, die wiederum das Postback verursacht hat. Ein kleiner Trick bietet jedoch eine problemumgehung für dieses Szenario an.

Erstens, sieht die grundlegende Einrichtung: zwei Textfelder, die beide die gleiche Popup, einen Kalender auslösen. Zwei `PopupControlExtenders` Versammeln Sie Textfelder und Popupfenster.

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample1.aspx)]

Die Grundidee ist ein ausgeblendetes Formularfeld im Hinzufügen der &lt; `form` &gt; Element, das das Textfeld enthält, der das Popup gestartet:

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample2.aspx)]

Wenn die Seite geladen ist, fügt JavaScript-Code einen Ereignishandler beide Textfelder: Wenn der Benutzer in einem Textfeld klickt, seinen Namen in der ausgeblendetes Formularfeld geschrieben:

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample3.html)]

In den serverseitigen Code muss der Wert des ausgeblendeten Felds gelesen werden. Da ausgeblendete Felder zum Bearbeiten von trivialen sind, ist ein Positivliste Ansatz zur Überprüfung des Werts der ausgeblendeten erforderlich. Nachdem die richtige Textfeld identifiziert wurde, wird das Datum im Kalender hinein geschrieben.

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/samples/sample4.aspx)]


[![Der Kalender wird angezeigt, wenn der Benutzer in das Textfeld klickt](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image1.png)

Der Kalender wird angezeigt, wenn der Benutzer in das Textfeld klickt ([klicken Sie hier, um das Bild in voller Größe angezeigt](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image3.png))


[![Durch Klicken auf ein Datum in das Textfeld eingefügt](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image4.png)

Durch Klicken auf ein Datum in das Textfeld eingefügt ([klicken Sie hier, um das Bild in voller Größe angezeigt](handling-postbacks-from-a-popup-control-without-an-updatepanel-vb/_static/image6.png))

> [!div class="step-by-step"]
> [Vorherige](handling-postbacks-from-a-popup-control-with-an-updatepanel-vb.md)
