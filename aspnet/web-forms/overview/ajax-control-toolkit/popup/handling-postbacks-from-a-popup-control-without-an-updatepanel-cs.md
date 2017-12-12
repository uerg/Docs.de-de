---
uid: web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
title: Behandlung von Postbacks aus einem Popupsteuerelement ohne UpdatePanel (c#) | Microsoft Docs
author: wenz
description: "Der Extender PopupControl im AJAX Control Toolkit bietet eine einfache Möglichkeit, um ein Popup auszulösen, wenn es sich bei jedem anderen Steuerelement aktiviert wird. Tritt ein Postback in \"su\"..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 25444121-5a72-4dac-8e50-ad2b7ac667af
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/popup/handling-postbacks-from-a-popup-control-without-an-updatepanel-cs
msc.type: authoredcontent
ms.openlocfilehash: df43052950b6186908fe1baf04808f40cb926f69
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 11/10/2017
---
<a name="handling-postbacks-from-a-popup-control-without-an-updatepanel-c"></a>Behandlung von Postbacks aus einem Popupsteuerelement ohne UpdatePanel (c#)
====================
durch [Christian Wenz](https://github.com/wenz)

[Herunterladen von Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/PopupControl3.cs.zip) oder [PDF herunterladen](http://download.microsoft.com/download/2/d/c/2dc10e34-6983-41d4-9c08-f78f5387d32b/popupcontrol3CS.pdf)

> Der Extender PopupControl im AJAX Control Toolkit bietet eine einfache Möglichkeit, um ein Popup auszulösen, wenn es sich bei jedem anderen Steuerelement aktiviert wird. Wenn ein Postback in einem solchen Bereich erfolgt und ggf. mehrere Bereiche auf der Seite werden ist es schwierig, zu bestimmen, welcher Bereich geklickt wurde.


## <a name="overview"></a>Übersicht

Der Extender PopupControl im AJAX Control Toolkit bietet eine einfache Möglichkeit, um ein Popup auszulösen, wenn es sich bei jedem anderen Steuerelement aktiviert wird. Wenn ein Postback in einem solchen Bereich erfolgt und ggf. mehrere Bereiche auf der Seite werden ist es schwierig, zu bestimmen, welcher Bereich geklickt wurde.

## <a name="steps"></a>Schritte

Bei Verwendung einer `PopupControl` mit einem Postback, aber ohne eine `UpdatePanel` auf der Seite die Control-Toolkit nicht bieten eine Möglichkeit zu bestimmen, welches Clientelement das Popup ausgelöst wurde, die wiederum das Postback verursacht hat. Ein kleiner Trick bietet jedoch eine problemumgehung für dieses Szenario an.

Erstens, sieht die grundlegende Einrichtung: zwei Textfelder, die beide die gleiche Popup, einen Kalender auslösen. Zwei `PopupControlExtenders` Versammeln Sie Textfelder und Popupfenster.

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample1.aspx)]

Die Grundidee ist ein ausgeblendetes Formularfeld im Hinzufügen der &lt; `form` &gt; Element, das das Textfeld enthält, der das Popup gestartet:

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample2.aspx)]

Wenn die Seite geladen ist, fügt JavaScript-Code einen Ereignishandler beide Textfelder: Wenn der Benutzer in einem Textfeld klickt, seinen Namen in der ausgeblendetes Formularfeld geschrieben:

[!code-html[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample3.html)]

In den serverseitigen Code muss der Wert des ausgeblendeten Felds gelesen werden. Da ausgeblendete Felder zum Bearbeiten von trivialen sind, ist ein Positivliste Ansatz zur Überprüfung des Werts der ausgeblendeten erforderlich. Nachdem die richtige Textfeld identifiziert wurde, wird das Datum im Kalender hinein geschrieben.

[!code-aspx[Main](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/samples/sample4.aspx)]


[![Der Kalender wird angezeigt, wenn der Benutzer in das Textfeld klickt](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image2.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image1.png)

Der Kalender wird angezeigt, wenn der Benutzer in das Textfeld klickt ([klicken Sie hier, um das Bild in voller Größe angezeigt](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image3.png))


[![Durch Klicken auf ein Datum in das Textfeld eingefügt](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image5.png)](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image4.png)

Durch Klicken auf ein Datum in das Textfeld eingefügt ([klicken Sie hier, um das Bild in voller Größe angezeigt](handling-postbacks-from-a-popup-control-without-an-updatepanel-cs/_static/image6.png))

>[!div class="step-by-step"]
[Zurück](handling-postbacks-from-a-popup-control-with-an-updatepanel-cs.md)
[Weiter](using-multiple-popup-controls-vb.md)
