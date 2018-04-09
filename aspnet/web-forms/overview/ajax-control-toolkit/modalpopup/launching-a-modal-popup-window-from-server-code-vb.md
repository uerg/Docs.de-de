---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
title: Starten ein modales Popupfenster von Servercode (VB) | Microsoft Docs
author: wenz
description: ModalPopup-Steuerelement in das AJAX-Steuerelement-Toolkit bietet eine einfache Möglichkeit, ein modales Popupdialogfeld clientseitige Weise zu erstellen. Einige Szenarien erfordern jedoch, t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 36ca81d7-906d-4db2-952b-add18a4ff421
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-vb
msc.type: authoredcontent
ms.openlocfilehash: 46554dae60ad9cd13e97e5755e95cb2125d1fed9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
---
<a name="launching-a-modal-popup-window-from-server-code-vb"></a>Starten ein modales Popupfenster von Servercode (VB)
====================
durch [Christian Wenz](https://github.com/wenz)

[Herunterladen von Code](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.vb.zip) oder [PDF herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1VB.pdf)

> ModalPopup-Steuerelement in das AJAX-Steuerelement-Toolkit bietet eine einfache Möglichkeit, ein modales Popupdialogfeld clientseitige Weise zu erstellen. Einige Szenarien erfordern jedoch, dass das Öffnen des modales Popupdialogfeld auf der Serverseite ausgelöst wird.


## <a name="overview"></a>Übersicht

ModalPopup-Steuerelement in das AJAX-Steuerelement-Toolkit bietet eine einfache Möglichkeit, ein modales Popupdialogfeld clientseitige Weise zu erstellen. Einige Szenarien erfordern jedoch, dass das Öffnen des modales Popupdialogfeld auf der Serverseite ausgelöst wird.

## <a name="steps"></a>Schritte

Erstens muss ASP.NET Web Schaltflächensteuerelement veranschaulichen die Funktionsweise der ModalPopup-Steuerelement. Fügen Sie eine solche Schaltfläche innerhalb der &lt;Formular&gt; Element auf einer neuen Seite:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample1.aspx)]

Klicken Sie dann, benötigen Sie das Markup für das Popupfenster, die, das Sie erstellen möchten. Definieren Sie ihn als eine `<asp:Panel>` steuern und stellen Sie sicher, dass sie ein Button-Steuerelement enthalten. Das Steuerelement ModalPopup bietet die Funktionalität, um eine solche Schaltfläche Schließen das Popup zu machen; Andernfalls ist keine einfache Methode zum verschwinden zu lassen.

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample2.aspx)]

Fügen Sie als Nächstes das ModalPopup-Steuerelement aus dem ASP.NET AJAX-Toolkit auf der Seite. Legen Sie Eigenschaften für die Schaltfläche, die das Steuerelement lädt, die Schaltfläche "", wodurch das nicht mehr angezeigt, und die tatsächliche Popup-ID an.

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample3.aspx)]

Wie bei allen Webseiten, die basierend auf ASP.NET AJAX; der Skript-Manager ist erforderlich, um die erforderlichen JavaScript-Bibliotheken für die anderen Ziel-Browser zu laden:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample4.aspx)]

Führen Sie das Beispiel im Browser. Wenn Sie auf die Schaltfläche klicken, wird die modale Popupdialogfeld angezeigt. Um den gleichen Effekt mit serverseitigen Code zu erreichen, ist eine Schaltfläche "Neu" erforderlich:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample5.aspx)]

Wie Sie sehen können, wird mit einem Klick auf die Schaltfläche einen Postback generiert und führt die `ServerButton_Click()` Methode auf dem Server. Bei dieser Methode wird eine JavaScript-Funktion wird aufgerufen, `launchModal()` wird ausgeführt, um genau zu sein, werden die JavaScript-Funktion ausgeführt, sobald die Seite geladen wurde:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample6.aspx)]

Die Aufgabe von `launchModal()` besteht darin, die ModalPopup anzuzeigen. Die `launchModal()` Funktion ausgeführt wird, sobald die vollständige HTML-Seite geladen wurden. Zu diesem Zeitpunkt jedoch wurden das ASP.NET AJAX-Framework vollständig geladen noch nicht. Aus diesem Grund die `launchModal()` Funktion wird nur eine Variable, die das Steuerelement ModalPopup später angezeigt werden muss:

[!code-html[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample7.html)]

Die `pageLoad()` JavaScript-Funktion ist eine spezielle Funktion, die ausgeführt wird, wenn ASP.NET AJAX vollständig geladen wurde. Daher wir diese Funktion zum Anzeigen der ModalPopup-Steuerelement, jedoch nur, wenn Code hinzufügen `launchModal()` vor aufgerufen wurde:

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-vb/samples/sample8.js)]

Die `$find()` -Funktion sucht nach einem benannten Element auf der Seite und die serverseitige-ID als Parameter erwartet. Aus diesem Grund `$find("mpe")` gibt die Clientdarstellung des Steuerelements ModalPopup zurück, dessen `show()` Methode ermöglicht das Popupfenster angezeigt werden.


[![Modale Popupdialogfeld angezeigt, wenn eine der Schaltflächen geklickt wird](launching-a-modal-popup-window-from-server-code-vb/_static/image2.png)](launching-a-modal-popup-window-from-server-code-vb/_static/image1.png)

Modale Popupdialogfeld angezeigt, wenn eine der Schaltflächen geklickt wird ([klicken Sie hier, um das Bild in voller Größe angezeigt](launching-a-modal-popup-window-from-server-code-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Zurück](positioning-a-modalpopup-cs.md)
> [Weiter](using-modalpopup-with-a-repeater-control-vb.md)
