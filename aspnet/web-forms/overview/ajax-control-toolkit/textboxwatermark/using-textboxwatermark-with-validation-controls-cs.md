---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
title: Verwenden von TextBoxWatermark mit Validierungssteuerelementen (c#) | Microsoft-Dokumentation
author: wenz
description: Das Steuerelement "TextBoxWatermark" im AJAX Control Toolkit erweitert ein Textfeld, sodass ein Text in das Feld angezeigt wird. Klickt ein Benutzer in das Feld, es ich...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: d49940cb-d38c-456a-b800-5f0eb705d09f
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-cs
msc.type: authoredcontent
ms.openlocfilehash: 35dd340ffce7b83303a44a0d6532ce73be5350f0
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41836221"
---
<a name="using-textboxwatermark-with-validation-controls-c"></a>Verwenden von TextBoxWatermark mit Validierungssteuerelementen (c#)
====================
durch [Christian Wenz](https://github.com/wenz)

[Code herunterladen](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.cs.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2CS.pdf)

> Das Steuerelement "TextBoxWatermark" im AJAX Control Toolkit erweitert ein Textfeld, sodass ein Text in das Feld angezeigt wird. Wenn ein Benutzer in das Feld klickt, wird es geleert. Wenn der Benutzer das Feld lässt sich ohne Eingabe von Text, wird der vorab ausgefüllte Text erneut. Dies möglicherweise in Konflikt mit ASP.NET-Validierungssteuerelementen auf derselben Seite, aber diese Probleme umgehen können.


## <a name="overview"></a>Übersicht

Die `TextBoxWatermark` Steuerelement im AJAX Control Toolkit erweitert ein Textfeld aus, sodass ein Text in das Feld angezeigt wird. Wenn ein Benutzer in das Feld klickt, wird es geleert. Wenn der Benutzer das Feld lässt sich ohne Eingabe von Text, wird der vorab ausgefüllte Text erneut. Dies möglicherweise in Konflikt mit ASP.NET-Validierungssteuerelementen auf derselben Seite, aber diese Probleme umgehen können.

## <a name="steps"></a>Schritte

Die grundlegende Einrichtung des Beispiels ist Folgendes: eine `TextBox` Steuerelement wird mit einem Wasserzeichen versehen mit einer `TextBoxWatermarkExtender` Steuerelement. Eine Schaltfläche löst einen Postback und später zum Auslösen der Validierungssteuerelemente auf der Seite verwendet werden. Darüber hinaus eine `ScriptManager` Steuerelement ist erforderlich, um ASP.NET AJAX zu initialisieren:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample1.aspx)]

Fügen Sie jetzt eine `RequiredFieldValidator` -Steuerelement, das überprüft, ob Text in das Feld vorhanden ist, wenn das Formular übermittelt wird. Die `InitialValue` Eigenschaft des Validierungssteuerelements muss festgelegt werden, auf den gleichen Wert, der verwendet wird die `TextBoxWatermarkExtender` Kontrolle: Wenn das Formular gesendet wird, ist der Wert des eine unverändert Textbox des Grenzwerts in diesem:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample2.aspx)]

Allerdings ist ein Problem bei diesem Ansatz: Wenn der Client über JavaScript deaktiviert, das Textfeld ist nicht bereits ausgefüllt ist mit der Wasserzeichentext aus diesem Grund die `RequiredFieldValidator` eine Fehlermeldung wird nicht ausgelöst. Aus diesem Grund eine zweite `RequiredFieldValidator` Steuerelement ist erforderlich, die überprüft, ob ein leeres Textfeld (das Auslassen der `InitialValue` Attribut).

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample3.aspx)]

Da beide Validierungssteuerelemente verwenden `Display` = `"Dynamic"`, der Endbenutzer kann nicht über die visuelle Darstellung der zwei Validatoren ausgelöst wurde. unterscheiden; stattdessen offenbar gab es nur eine davon.

Abschließend fügen Sie serverseitigen Code, um den Text im Feld ausgeben, wenn kein Validierungssteuerelement eine Fehlermeldung ausgegeben:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-cs/samples/sample4.aspx)]


[![Das Validierungssteuerelement meldet, dass es in das Feld kein Text ist](using-textboxwatermark-with-validation-controls-cs/_static/image2.png)](using-textboxwatermark-with-validation-controls-cs/_static/image1.png)

Das Validierungssteuerelement meldet, dass kein Text vorhanden, in das Feld ist ([klicken Sie, um das Bild in voller Größe anzeigen](using-textboxwatermark-with-validation-controls-cs/_static/image3.png))

> [!div class="step-by-step"]
> [Zurück](using-textboxwatermark-in-a-formview-cs.md)
> [Weiter](using-textboxwatermark-in-a-formview-vb.md)
