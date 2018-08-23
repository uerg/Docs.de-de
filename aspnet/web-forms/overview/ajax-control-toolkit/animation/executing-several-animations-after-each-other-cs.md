---
uid: web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
title: Ausführen mehrerer Animationen nach anderen (c#) | Microsoft-Dokumentation
author: wenz
description: Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Er ermöglicht es, durch Fallenlassen ausführen...
ms.author: riande
ms.date: 06/02/2008
ms.assetid: 7dc02b18-2b5d-4844-b7c5-cbd818477163
msc.legacyurl: /web-forms/overview/ajax-control-toolkit/animation/executing-several-animations-after-each-other-cs
msc.type: authoredcontent
ms.openlocfilehash: 60fb7dacfe59eaafef71fcc9cde61b7840624343
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: de-DE
ms.lasthandoff: 08/16/2018
ms.locfileid: "41828099"
---
<a name="executing-several-animations-after-each-other-c"></a>Ausführen mehrerer Animationen nach anderen (c#)
====================
durch [Christian Wenz](https://github.com/wenz)

[Code herunterladen](http://download.microsoft.com/download/f/9/a/f9a26acd-8df4-4484-8a18-199e4598f411/Animation3.cs.zip) oder [PDF-Datei herunterladen](http://download.microsoft.com/download/6/7/1/6718d452-ff89-4d3f-a90e-c74ec2d636a3/animation3CS.pdf)

> Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Sie können zum Ausführen mehrerer Animationen eine nach dem anderen.


Die Animation-Steuerelement in ASP.NET AJAX Control Toolkit ist nicht nur ein Steuerelement, aber ein ganzes Framework Animationen an ein Steuerelement hinzufügen. Sie können zum Ausführen mehrerer Animationen eine nach dem anderen.

## <a name="steps"></a>Schritte

Zunächst einmal sind die `ScriptManager` in die Seite klicken Sie dann die ASP.NET AJAX-Bibliothek wird geladen, lässt sich das Steuerelement-Toolkit verwenden:

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample1.aspx)]

Die Animation wird auf einen Bereich des Texts angewendet werden, der so aussieht:

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample2.aspx)]

Definieren Sie in der zugehörigen CSS-Klasse für den Bereich eine gute Hintergrundfarbe aus, und auch festlegen Sie eine feste Breite für den Bereich:

[!code-css[Main](executing-several-animations-after-each-other-cs/samples/sample3.css)]

Fügen Sie dann die `AnimationExtender` auf der Seite Bereitstellen einer `ID`, `TargetControlID` -Attribut und das obligatorische `runat="server":`

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample4.aspx)]

In der `<Animations>` Knoten verwenden `<OnLoad>` die Animationen ausgeführt werden, nachdem die Seite vollständig geladen wurde. Im allgemeinen `<OnLoad>` akzeptiert nur eine Animation. Der Animation-Framework können Sie mehrere Animationen in einer mit join die `<Sequence>` Element. Alle Animationen in `<Sequence>` sind, werden nach dem anderen. Hier ist die einem möglichen Markup für die `AnimationExtender` Steuerelement, sodass zuerst den größeren Bereich und dann Verringern der Höhe:

[!code-aspx[Main](executing-several-animations-after-each-other-cs/samples/sample5.aspx)]

Wenn Sie dieses Skript im Bereich erste ruft breiter und dann kleinere ausführen.


[![Zuerst wird die Breite erhöht.](executing-several-animations-after-each-other-cs/_static/image2.png)](executing-several-animations-after-each-other-cs/_static/image1.png)

Zuerst wird die Breite erhöht ([klicken Sie, um das Bild in voller Größe anzeigen](executing-several-animations-after-each-other-cs/_static/image3.png))


[![Anschließend wird die Höhe verringert.](executing-several-animations-after-each-other-cs/_static/image5.png)](executing-several-animations-after-each-other-cs/_static/image4.png)

Und dann die Höhe verringert wird ([klicken Sie, um das Bild in voller Größe anzeigen](executing-several-animations-after-each-other-cs/_static/image6.png))

> [!div class="step-by-step"]
> [Zurück](executing-several-animations-at-the-same-time-cs.md)
> [Weiter](animation-depending-on-a-condition-cs.md)
