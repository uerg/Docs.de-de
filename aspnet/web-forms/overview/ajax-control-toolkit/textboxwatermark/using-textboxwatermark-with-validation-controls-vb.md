---
uid: web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-vb
title: Validierungssteuerelemente (VB) TextBoxWatermark mit | Microsoft Docs
author: wenz
description: TextBoxWatermark-Steuerelements in der AJAX-Steuerelement-Toolkit erweitert ein Textfeld, sodass ein Text in das Feld angezeigt wird. Klickt ein Benutzer in das Feld es ich...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/02/2008
ms.topic: article
ms.assetid: e6c2cb98-f745-4bc8-973a-813879c8a891
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/textboxwatermark/using-textboxwatermark-with-validation-controls-vb
msc.type: authoredcontent
ms.openlocfilehash: 0ca1a4af62af1d65525e59d0b7bc47245dd01476
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879230"
---
<a name="using-textboxwatermark-with-validation-controls-vb"></a>Verwenden von TextBoxWatermark mit Validierungssteuerelemente (VB)
====================
durch [Christian Wenz](https://github.com/wenz)

[Herunterladen von Code](http://download.microsoft.com/download/9/3/f/93f8daea-bebd-4821-833b-95205389c7d0/TextBoxWatermark2.vb.zip) oder [PDF herunterladen](http://download.microsoft.com/download/b/6/a/b6ae89ee-df69-4c87-9bfb-ad1eb2b23373/textboxwatermark2VB.pdf)

> TextBoxWatermark-Steuerelements in der AJAX-Steuerelement-Toolkit erweitert ein Textfeld, sodass ein Text in das Feld angezeigt wird. Klickt ein Benutzer in das Feld, wird es geleert. Wenn der Benutzer das Feld verlässt, ohne Text eingeben, wird das vorab ausgefüllte Text erneut. Dies kann einen Konflikt mit ASP.NET-Validierungssteuerelementen auf derselben Seite verursachen, aber diese Probleme umgehen können.


## <a name="overview"></a>Übersicht

Die `TextBoxWatermark` Steuerelement im AJAX-Steuerelement-Toolkit erweitert ein Textfeld aus, sodass ein Text in das Feld angezeigt wird. Klickt ein Benutzer in das Feld, wird es geleert. Wenn der Benutzer das Feld verlässt, ohne Text eingeben, wird das vorab ausgefüllte Text erneut. Dies kann einen Konflikt mit ASP.NET-Validierungssteuerelementen auf derselben Seite verursachen, aber diese Probleme umgehen können.

## <a name="steps"></a>Schritte

Einfache Einrichtung des Beispiels lautet wie folgt: ein `TextBox` Steuerelement ist mit einem Wasserzeichen versehen mithilfe einer `TextBoxWatermarkExtender` Steuerelement. Eine Schaltfläche löst einen Postback und später zum Auslösen der Validierungssteuerelemente auf der Seite verwendet werden. Darüber hinaus eine `ScriptManager` -Steuerelement ist erforderlich, um ASP.NET AJAX zu initialisieren:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample1.aspx)]

Fügen Sie jetzt eine `RequiredFieldValidator` Steuerelement, das überprüft, ob Text im Feld vorhanden ist, wenn das Formular gesendet wird. Die `InitialValue` das Validierungssteuerelement Eigenschaft muss festgelegt werden, auf den gleichen Wert, der verwendet wird die `TextBoxWatermarkExtender` Steuerelement: beim Senden des Formulars wird der Wert von einer unveränderten Textfeld Wert für die Wasserzeichen darin:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample2.aspx)]

Allerdings ist ein Problem bei diesem Ansatz: Wenn der Client, JavaScript deaktiviert, das Textfeld "ist nicht ausgefüllt mit Wasserzeichentexts, daher die `RequiredFieldValidator` eine Fehlermeldung wird nicht ausgelöst. Aus diesem Grund eine zweite `RequiredFieldValidator` Control ist erforderlich, die überprüft, ob leere Textfelder (Auslassen der `InitialValue` Attribut).

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample3.aspx)]

Da beide Validierungssteuerelemente verwenden `Display` = `"Dynamic"`, der Endbenutzer kann nicht über die visuelle Darstellung, die die zwei Validierungssteuerelemente ausgelöst wurde unterscheiden; stattdessen scheint gab es nur eine Kopie.

Fügen Sie schließlich serverseitigen Code zum den Text im Feld ausgegeben, wenn kein Validierungssteuerelement eine Fehlermeldung ausgegeben:

[!code-aspx[Main](using-textboxwatermark-with-validation-controls-vb/samples/sample4.aspx)]


[![Die Bestätigung meldet, dass es in das Feld kein Text ist](using-textboxwatermark-with-validation-controls-vb/_static/image2.png)](using-textboxwatermark-with-validation-controls-vb/_static/image1.png)

Die Bestätigung meldet, dass es in das Feld kein Text ist ([klicken Sie hier, um das Bild in voller Größe angezeigt](using-textboxwatermark-with-validation-controls-vb/_static/image3.png))

> [!div class="step-by-step"]
> [Vorherige](using-textboxwatermark-in-a-formview-vb.md)
