---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-vb
title: Verwenden von TextBoxWatermark mit Validierungssteuerelementen (VB) | Microsoft-Dokumentation
author: wenz
description: Das Steuerelement "TextBoxWatermark" im AJAX Control Toolkit erweitert ein Textfeld, sodass ein Text in das Feld angezeigt wird. Klickt ein Benutzer in das Feld, es ich...
ms.author: aspnetcontent
ms.date: 06/02/2008
ms.assetid: e6c2cb98-f745-4bc8-973a-813879c8a891
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 67e7a715545b35c25147c6dbf6684e0e1634027b
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 07/05/2018
ms.locfileid: "37811564"
---
<a name="using-textboxwatermark-with-validation-controls-vb"></a>Verwenden von TextBoxWatermark mit Validierungssteuerelementen (VB)
====================
durch [Christian Wenz](https://github.com/wenz)

[Code herunterladen](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.vb.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2VB.pdf)

> Das Steuerelement "TextBoxWatermark" im AJAX Control Toolkit erweitert ein Textfeld, sodass ein Text in das Feld angezeigt wird. Wenn ein Benutzer in das Feld klickt, wird es geleert. Wenn der Benutzer das Feld lässt sich ohne Eingabe von Text, wird der vorab ausgefüllte Text erneut. Dies möglicherweise in Konflikt mit ASP.NET-Validierungssteuerelementen auf derselben Seite, aber diese Probleme umgehen können.


## <a name="overview"></a>Übersicht

Die `TextBoxWatermark` Steuerelement im AJAX Control Toolkit erweitert ein Textfeld aus, sodass ein Text in das Feld angezeigt wird. Wenn ein Benutzer in das Feld klickt, wird es geleert. Wenn der Benutzer das Feld lässt sich ohne Eingabe von Text, wird der vorab ausgefüllte Text erneut. Dies möglicherweise in Konflikt mit ASP.NET-Validierungssteuerelementen auf derselben Seite, aber diese Probleme umgehen können.

## <a name="steps"></a>Schritte

Die grundlegende Einrichtung des Beispiels ist Folgendes: eine `TextBox` Steuerelement wird mit einem Wasserzeichen versehen mit einer `TextBoxWatermarkExtender` Steuerelement. Eine Schaltfläche löst einen Postback und später zum Auslösen der Validierungssteuerelemente auf der Seite verwendet werden. Darüber hinaus eine `ScriptManager` Steuerelement ist erforderlich, um ASP.NET AJAX zu initialisieren:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample1.aspx)]

Fügen Sie jetzt eine `RequiredFieldValidator` -Steuerelement, das überprüft, ob Text in das Feld vorhanden ist, wenn das Formular übermittelt wird. Die `InitialValue` Eigenschaft des Validierungssteuerelements muss festgelegt werden, auf den gleichen Wert, der verwendet wird die `TextBoxWatermarkExtender` Kontrolle: Wenn das Formular gesendet wird, ist der Wert des eine unverändert Textbox des Grenzwerts in diesem:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample2.aspx)]

Allerdings ist ein Problem bei diesem Ansatz: Wenn der Client über JavaScript deaktiviert, das Textfeld ist nicht bereits ausgefüllt ist mit der Wasserzeichentext aus diesem Grund die `RequiredFieldValidator` eine Fehlermeldung wird nicht ausgelöst. Aus diesem Grund eine zweite `RequiredFieldValidator` Steuerelement ist erforderlich, die überprüft, ob ein leeres Textfeld (das Auslassen der `InitialValue` Attribut).

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample3.aspx)]

Da beide Validierungssteuerelemente verwenden `Display` = `"Dynamic"`, der Endbenutzer kann nicht über die visuelle Darstellung der zwei Validatoren ausgelöst wurde. unterscheiden; stattdessen offenbar gab es nur eine davon.

Abschließend fügen Sie serverseitigen Code, um den Text im Feld ausgeben, wenn kein Validierungssteuerelement eine Fehlermeldung ausgegeben:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample4.aspx)]


[![Das Validierungssteuerelement meldet, dass es in das Feld kein Text ist](using-textboxwatermark-with-validation-controls-vb/_static/image2.png)](using-textboxwatermark-with-validation-controls-vb/_static/image1.png)

Das Validierungssteuerelement meldet, dass kein Text vorhanden, in das Feld ist ([klicken Sie, um das Bild in voller Größe anzeigen](using-textboxwatermark-with-validation-controls-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Vorherige](using-textboxwatermark-in-a-formview-vb.md)
