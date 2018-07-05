---
uid: web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-cs
title: Starten eines modalen Popupfensters über den Servercode (c#) | Microsoft-Dokumentation
author: wenz
description: Der ModalPopup-Steuerelement im AJAX Control Toolkit bietet eine einfache Möglichkeit, ein modales Fenster mithilfe der clientseitigen Methoden zu erstellen. Einige Szenarien erfordern jedoch, t...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: 2f67d8ef-73ca-447d-a0cc-6e3168431e6a
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/modalpopup/launching-a-modal-popup-window-from-server-code-cs
msc.type: authoredcontent
ms.openlocfilehash: a13915f5748f8ad9b79fb787cc5e0682faa19388
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/03/2018
ms.locfileid: "37374090"
---
<a name="launching-a-modal-popup-window-from-server-code-c"></a>Starten eines modalen Popupfensters über den Servercode (c#)
====================
durch [Christian Wenz](https://github.com/wenz)

[Code herunterladen](http://download.microsoft.com/download/2/4/0/24052038-f942-4336-905b-b60ae56f0dd5/ModalPopup1.cs.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/modalpopup1CS.pdf)

> Der ModalPopup-Steuerelement im AJAX Control Toolkit bietet eine einfache Möglichkeit, ein modales Fenster mithilfe der clientseitigen Methoden zu erstellen. Einige Szenarien erfordern jedoch, dass das Öffnen eines modalen Popups auf der Serverseite ausgelöst wird.


## <a name="overview"></a>Übersicht

Der ModalPopup-Steuerelement im AJAX Control Toolkit bietet eine einfache Möglichkeit, ein modales Fenster mithilfe der clientseitigen Methoden zu erstellen. Einige Szenarien erfordern jedoch, dass das Öffnen eines modalen Popups auf der Serverseite ausgelöst wird.

## <a name="steps"></a>Schritte

Zunächst muss eine ASP.NET-Schaltfläche-Websteuerelement zur Veranschaulichung der Funktionsweise des ModalPopup-Steuerelements. Fügen Sie eine solche Schaltfläche innerhalb der &lt;Formular&gt; Element auf einer neuen Seite:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample1.aspx)]

Klicken Sie dann benötigen Sie das Markup für das Popup, die, das Sie erstellen möchten. Definieren ihn als einen `<asp:Panel>` steuern, und stellen Sie sicher, dass sie ein Schaltflächen-Steuerelement enthält. Der ModalPopup-Steuerelement bietet die Funktionalität, um eine solche Schaltfläche Schließen des Popups stellen; Andernfalls ist es keine einfache Möglichkeit, die sie verschwinden zu lassen.

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample2.aspx)]

Als Nächstes können Sie dem ModalPopup-Steuerelement aus dem ASP.NET AJAX-Toolkit auf der Seite hinzu. Legen Sie Eigenschaften für die Schaltfläche, die das Steuerelement geladen, die Taste, die sie nicht mehr angezeigt wird und die ID des tatsächlichen Popups.

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample3.aspx)]

Wie bei allen Webseiten, die basierend auf ASP.NET AJAX; der Skript-Manager erforderlich, um die erforderlichen JavaScript-Bibliotheken für die anderen Browser zu laden:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample4.aspx)]

Führen Sie das Beispiel im Browser. Wenn Sie auf die Schaltfläche klicken, wird das modale Popup angezeigt. Um den gleichen Effekt mit serverseitigem Code zu erreichen, ist eine neue Schaltfläche erforderlich:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample5.aspx)]

Wie Sie sehen können, ein Klick auf die Schaltfläche mit den generiert ein Postback und führt die `ServerButton_Click()` Methode auf dem Server. Bei dieser Methode eine JavaScript-Funktion wird aufgerufen, `launchModal()` wird ausgeführt, um genau zu sein, die JavaScript-Funktion ausgeführt wird, sobald die Seite geladen wurde:

[!code-aspx[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample6.aspx)]

Die Aufgabe des `launchModal()` besteht darin, den ModalPopup-Steuerelements anzuzeigen. Die `launchModal()` Funktion ausgeführt wird, nachdem die vollständige HTML-Seite geladen wurde. In diesem Moment aber wurde ASP.NET AJAX-Framework vollständig geladen noch nicht. Aus diesem Grund die `launchModal()` Funktion wird nur eine Variable, die das ModalPopup-Steuerelement muss einem späteren Zeitpunkt angezeigt werden:

[!code-html[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample7.html)]

Die `pageLoad()` JavaScript-Funktion ist eine spezielle Sigmoidfunktion, die ausgeführt wird, nachdem ASP.NET AJAX vollständig geladen wurde. Aus diesem Grund fügen wir Code für diese Funktion zum Anzeigen der ModalPopup-Steuerelement, aber nur, wenn `launchModal()` vor aufgerufen wurde:

[!code-javascript[Main](launching-a-modal-popup-window-from-server-code-cs/samples/sample8.js)]

Die `$find()` -Funktion sucht nach einem benannten Element auf der Seite und die serverseitige-ID als Parameter erwartet. Aus diesem Grund `$find("mpe")` gibt die Clientdarstellung des Steuerelements "ModalPopup" zurück, dessen `show()` -Methode können Sie das Popup angezeigt werden.


[![Modale Fenster wird angezeigt, wenn einer der Schaltflächen geklickt wird](launching-a-modal-popup-window-from-server-code-cs/_static/image2.png)](launching-a-modal-popup-window-from-server-code-cs/_static/image1.png)

Modale Fenster wird angezeigt, wenn einer der Schaltflächen geklickt wird ([klicken Sie, um das Bild in voller Größe anzeigen](launching-a-modal-popup-window-from-server-code-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Nächste](using-modalpopup-with-a-repeater-control-cs.md)
